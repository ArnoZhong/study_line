```java
package org.example.hdfs001;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.*;
import org.apache.hadoop.io.IOUtils;

import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.Iterator;
import java.util.Map;

/**
 * 创建一个测试类
 */
public class Test {
    private static FileSystem fs = null;

    public Test() throws InterruptedException, IOException, URISyntaxException {
        fs = get_file_system_uri();
    }

    /**
     * 初始化 HDFS
     * 获取 HDFS 操作对象 根据给定的 uri 远程访问配置文件常见 HDFS 对象
     * 适用于本地没有 Hadoop 系统
     *
     * @return
     * @throws URISyntaxException
     * @throws IOException
     * @throws InterruptedException
     */
    public static FileSystem get_file_system_uri() throws URISyntaxException, IOException, InterruptedException {
        Configuration conf = new Configuration();
        URI hdfs_uri = new URI("hdfs://cdh01:9000");
        FileSystem fs = FileSystem.get(hdfs_uri, conf);
        return fs;
    }

    /**
     * 初始化 DHFS
     * 获取 HDFS 操作对象，根据本地 HDFS 配置信息
     * 适用于本地有 Hadoop 系统
     *
     * @return
     * @throws IOException
     */
    public static FileSystem get_file_system_con() throws IOException {
        Configuration conf = new Configuration();
        String conf_name = "fs.defaultFS";
        String conf_value = "hdfs://mycluster";
        conf.set(conf_name, conf_value);
        FileSystem fs = FileSystem.get(conf);
        return fs;
    }

    /**
     * 在 HDFS 中创建文件夹
     *
     * @param hdfs_path
     * @throws InterruptedException
     * @throws IOException
     * @throws URISyntaxException
     */
    public void creat_path(String hdfs_path) throws InterruptedException, IOException, URISyntaxException {
        FileSystem hdfs = fs;
        Path path = new Path(hdfs_path);
        boolean b = hdfs.mkdirs(path);
        if (b) {
            System.out.println("Hdfs 文件夹创建成功");
        } else {
            System.out.println("Hdfs 文件夹创建失败");
        }
    }

    /**
     * 删除文件/文件夹
     *
     * @param hdfs_path
     * @throws IOException
     */
    public void delete_path(String hdfs_path) throws IOException {
        FileSystem hdfs = fs;
        Path detele_path = new Path(hdfs_path);
        boolean b = hdfs.delete(detele_path, true);
        if (b) {
            System.out.println("Hdfs 删除成功");
        } else {
            System.out.println("Hdfs 删除失败");
        }
    }

    /**
     * 文件夹重命名
     *
     * @param old_hdfs_path
     * @param new_hdfs_path
     * @throws IOException
     */
    public void rename_path(String old_hdfs_path, String new_hdfs_path) throws IOException {
        FileSystem hdfs = fs;
        Path old_path = new Path(old_hdfs_path);
        Path new_path = new Path(new_hdfs_path);
        boolean b = hdfs.rename(old_path, new_path);
        if (b) {
            System.out.println("Hdfs 文件夹重命名成功,结果为：" + new_path);
        } else {
            System.out.println("Hdfs 文件夹重命名失败");
        }
    }

    /**
     * 遍历查看文件夹下的所有内容
     *
     * @param hdfs_path 要遍历的文件路径
     * @throws IOException
     */
    public void read_path(String hdfs_path) throws IOException {
        FileSystem hdfs = fs;
        Path path = new Path(hdfs_path);
        FileStatus[] files = hdfs.listStatus(path);
        if (files.length == 0) {
            System.out.println("空文件夹：" + path.toUri().getPath());
        } else {
            for (FileStatus f : files) {
                if (files.length == 0 || f.isFile()) {
                    System.out.println("文件：" + f.getPath().toUri().getPath());
                } else {
                    read_path(f.getPath().toUri().getPath());
                }
            }
        }
    }

    /**
     * 判断给定路径是否存才，是否为文件或者文件夹
     *
     * @param hdfs_path
     * @throws IOException
     */
    public void check_parh(String hdfs_path) throws IOException {
        FileSystem hdfs = fs;
        Path path = new Path(hdfs_path);
        if (hdfs.exists(path)) {
            if (hdfs.isFile(path)) {
                System.out.println(hdfs_path + "：为文件");
            } else if (hdfs.isDirectory(path)) {
                System.out.println(hdfs_path + "：为文件夹");
            } else {
                System.out.println(hdfs_path + "：未知");
            }
        } else {
            System.out.println(hdfs_path + "：路径不存在");
        }
    }

    /**
     * 将本地文件上传到 HDFS
     *
     * @param file_path
     * @param hdfs_path
     * @throws IOException
     */
    public void cope_localfile_to_hdfs(String file_path, String hdfs_path) throws IOException {
        FileSystem hdfs = fs;
        Path hdfs_file_path = new Path(hdfs_path);
        Path local_file_path = new Path(file_path);
        hdfs.copyFromLocalFile(local_file_path, hdfs_file_path);
    }

    /**
     * 将文件从 hdfs 下载到本地
     *
     * @param local_file_path
     * @param hdfs_file_path
     * @throws IOException
     */
    public void copy_hdfs_to_local(String local_file_path, String hdfs_file_path) throws IOException {
        FileSystem hdfs = fs;
        Path local_path = new Path(local_file_path);
        Path hdfs_path = new Path(hdfs_file_path);
        hdfs.copyToLocalFile(hdfs_path, local_path);

    }

    /**
     * 将hdfs文件复制到hdfs 其他位置
     *
     * @param hdfs_file_path
     * @param new_hdfs_file_path
     * @throws IOException
     */
    public void copy_hdfs_to_hdfs(String hdfs_file_path, String new_hdfs_file_path) {
        FileSystem hdfs = fs;
        Path old_path = new Path(hdfs_file_path);
        Path new_path = new Path(new_hdfs_file_path);
        try {
            FSDataInputStream hdfs_in = hdfs.open(old_path);
            FSDataOutputStream hdfs_out = hdfs.create(new_path);
            IOUtils.copyBytes(hdfs_in, hdfs_out, 1024 * 1024 * 64, false);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 配置文件查看
     *
     * @param
     * @throws InterruptedException
     * @throws IOException
     * @throws URISyntaxException
     */
    public void show_all_conf() {
        FileSystem hdfs = fs;
        Configuration conf = hdfs.getConf();
        Iterator<Map.Entry<String, String>> it = conf.iterator();
        while (it.hasNext()) {
            Map.Entry<String, String> entry = it.next();
            System.out.println(entry.getKey() + "=" + entry.getValue());
        }
    }


    public static void main(String[] args) throws InterruptedException, IOException, URISyntaxException {
//         测试获取 Hdfs 对象是否成功
//        get_file_system_uri();
        Test hdfs_test = new Test();
        String path = "/test/java/test3";
//        hdfs_test.creat_path(path);
//        String localfilepath = "/Users/Arno/Documents/Arno/机器学习/book/Google云计算三大论文英文版.rar";
//        String hdfsfilepath = "/test/java/test1/";
//        hdfs_test.cope_localfile_to_hdfs(localfilepath,hdfsfilepath);

//        String localfilepath_new = "/Users/Arno/Documents/临时文件";
        String hdfsfilepath_new = "/test/java/test1/Google云计算三大论文英文版.rar";
//        hdfs_test.copy_hdfs_to_local(localfilepath_new, hdfsfilepath_new);
//        hdfs_test.read_path("/test");
//        hdfs_test.check_parh(path);
//        hdfs_test.check_parh(hdfsfilepath_new);
//        hdfs_test.show_all_conf();
//        hdfs_test.copy_hdfs_to_hdfs(hdfsfilepath_new,"/test/java/test1/google/Google云计算三大论文英文版.rar");
//        hdfs_test.read_path("/test/java/test1");
        hdfs_test.delete_path("/test/java/test1/Google云计算三大论文英文版.rar");
        hdfs_test.read_path("/test/java/test1");

    }
}

```

