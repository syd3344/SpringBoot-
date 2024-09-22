## 阿里云

> - 下载SDK依赖
> - 若是Java9以上的版本还要添加JAXB相关依赖

```xml
   <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
            <version>3.17.4</version>
        </dependency>
        <!-- no more than 2.3.3-->

        <dependency>
            <groupId>org.glassfish.jaxb</groupId>
            <artifactId>jaxb-runtime</artifactId>
            <version>2.3.3</version>
        </dependency>

        <dependency>
            <groupId>javax.xml.bind</groupId>
            <artifactId>jaxb-api</artifactId>
            <version>2.3.1</version>
        </dependency>
        <dependency>
            <groupId>javax.activation</groupId>
            <artifactId>activation</artifactId>
            <version>1.1.1</version>
        </dependency>
```





> - 核心配置信息
>   - 服务器地址：endpoint
>   - 身份标识：accessKeyID + accessKey Secret
>   - BucketName



## **使用MultipartFile实现**

```java
//1、配置基础信息
        // 服务器在哪
       // String endpoint = "";
        //身份信息
        //String accessKeyID = "";
       // String accessKeySecret="";
        // 填写Bucket名称
       // String bucketName = "";

         //生成唯一不重复名称
       // String originalFilename = image.getOriginalFilename(); //获取原始文件名
       // String type = originalFilename.substring(originalFilename.lastIndexOf("."));
       // String newFileName = UUID.randomUUID().toString()+type;


//2、核心代码--创建OSSClient实例
        OSS ossClient = new OSSClientBuilder().build( endpoint,accessKeyID,accessKeySecret );

        

//3、文件上传的核心方法--参数：存储空间，文件保存名称，待上传的文件数据
		try {
            ossClient.putObject(bucketName,newFileName ,image.getInputStream() );
        } catch (Exception oe) {

        } finally {
            if (ossClient != null) {
                ossClient.shutdown();
            }
        }


//4、返回地址 —— 用于回滚
        return Result.success("https://"+ bucketName+"."+ endpoint+"/"+ newFileName);
    }
```



## 使用工具类注入

### 1、工具类

```java
import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.io.InputStream;
import java.util.UUID;


@Component
public class ALiOssUtils {
    /**
     * 阿里云 OSS 工具类
     */
   // private String endpoint = "";
   // private String accessKeyId = "";
   // private String accessKeySecret = "";
  //  private String bucketName = "";

    /**
     * 实现上传图片到OSS
     */
    public String upload(MultipartFile file) throws IOException {
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));

        //上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        //文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭ossClient
        ossClient.shutdown();
        return url;// 把上传到oss的路径返回
    }

}

```



### 2、注入使用

```java
 @Autowired
    ALiOssUtils aLiOssUtils;


    @PostMapping("/upload")
        public Result upload(MultipartFile image) throws IOException {
        String url = aLiOssUtils.upload(image);
        return Result.success(url);
        }

```

