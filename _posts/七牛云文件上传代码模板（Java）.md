---
title: 七牛云文件上传代码模板（Java）
top_img: https://w.wallhaven.cc/full/yx/wallhaven-yx76vg.jpg
cover: https://w.wallhaven.cc/full/6d/wallhaven-6dqemx.jpg
abbrlink: 七牛云文件上传代码模板（Java）
tags:
    - JAVA
    - 七牛云
categories:
    - 杂七杂八

date: 2022/10/28 23:46:28 
---

# 下载依赖
~~~xml
<!--七牛云上传 java-sdk-->
<dependency>
    <groupId>com.qiniu</groupId>
    <artifactId>qiniu-java-sdk</artifactId>
    <version>[7.7.0, 7.10.99]</version>
</dependency>
~~~

# 上传、下载代码模板
~~~java
@SpringBootTest
@Data
@Component
@ConfigurationProperties("oss")
public class UploadTest {

    // 上传凭证 与 仓库名
    // 在 application.yml 配置文件修改
    private String accessKey;   // access key
    private String secretKey;   // secret key
    private String bucket;  // bucket name

    @Test
    public void testDownloadFile(String key){ // 下载测试
        String domain = "qiniu.heyzqf.com"; // 自定义域名

        // domain   下载 domain, eg: qiniu.com【必须】
        // useHttps 是否使用 https【必须】
        // key      下载资源在七牛云存储的 key【必须】
        DownloadUrl url = new DownloadUrl(domain, false, key);
        url.setAttname("TestPIC.png"); // 设置下载文件的 文件名

        try {
            String urlString = url.buildURL();
            System.out.println(urlString);
        } catch (QiniuException e) {
            e.printStackTrace();
        }
    }

    // @Test
    public void testReadFileDir(){ // 文件夹上传
        String path = "C:\\Users\\zqf\\Desktop\\1\\Blog\\source\\img\\PIC";
        File dir = new File(path);
        File[] listFiles = dir.listFiles();

        for(File file : listFiles){
            inputStreamUpload(file.getAbsolutePath());
        }
    }

    // @Test
    public void inputStreamUpload(String path){   // InputStream上传测试
        // 地区
        Region regionHuaNan = Region.huanan();
        // 构造一个带指定 Region 对象的配置类
        Configuration cfg = new Configuration(regionHuaNan);
        cfg.resumableUploadAPIVersion = Configuration.ResumableUploadAPIVersion.V2;// 指定分片上传版本

        UploadManager uploadManager = new UploadManager(cfg);

        // 默认不指定key的情况下，以文件内容的hash值作为文件名

        String key = "测试目录/"+PathUtils.generateFilePath(path);

        try {
            // 如果是Windows情况下，格式是 D:\\qiniu\\test.png
            String localFilePath = path;
            InputStream inputStream = new FileInputStream(localFilePath);
            Auth auth = Auth.create(accessKey, secretKey);
            String upToken = auth.uploadToken(bucket);

            Response response = null;
            DefaultPutRet putRet = null;
            try {
                System.out.println("上传文件： " + localFilePath);
                response = uploadManager.put(inputStream, key, upToken, null, null);
                // 解析上传成功的结果
                putRet = new Gson().fromJson(response.bodyString(), DefaultPutRet.class);
                System.out.println("putRet.key: " + putRet.key);
                System.out.println("putRet.hash: " + putRet.hash);
            } catch (QiniuException ex) {
                ex.printStackTrace();
                Response r = ex.response;
                System.err.println(r.toString());
                try {
                    System.err.println(r.bodyString());
                } catch (QiniuException e2) {
                    e2.printStackTrace();
                    //ignore
                }
            }
            System.out.println("上传成功：");
            System.out.println("    response: " + response);
            System.out.println("    putRet: " + putRet);
            System.out.println("********************************************************************************************************************************************************************************************************************************************************************************************************************");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
~~~

# PathUtils 路径工具类
~~~java
public class PathUtils {    // 文件路径生成 工具类

    public static String generateFilePath(String fileName){
        // 根据日期生成路径   2022/10/31/
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd/");
        String datePath = dateFormat.format(new Date());

        // 随机生成UUID 作为文件名
        String uuid = UUID.randomUUID().toString().replace("-", "");
        // 获取文件原后缀名，使新文件名的后缀与原文件一样
        int index = fileName.lastIndexOf(".");
        // test.jpg -> .jpg
        String fileType = fileName.substring(index);
        // 拼接字符串返回
        return new StringBuilder().append(datePath).append(uuid).append(fileType).toString();

    }
}
~~~
