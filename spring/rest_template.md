## 示例

<!-- toc -->

### 直接传递 JSON 字符串


```java
import com.alibaba.fastjson.JSONObject;
import org.junit.Test;
import org.springframework.http.*;
import org.springframework.web.client.RestTemplate;

/**
 * @author Jikai Zhang
 * @date 2018-04-16
 */
public class Test {

    @Test
    public void test() {
        JSONObject requestJSON = new JSONObject();
        requestJSON.put("1111", "222");
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        HttpEntity<String> requestEntity = new HttpEntity<>(requestJSON.toJSONString(), headers);

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.exchange("http://api.test.com", HttpMethod.POST, requestEntity, String.class);
        System.out.println(response.getStatusCode() + " " + response.getBody());
    }
}
```


### 模拟表单提交

```java
import org.junit.Test;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;
import org.springframework.http.*;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.client.RestTemplate;

import java.io.File;
import java.util.Date;
import java.util.Random;

/**
 * @author Jikai Zhang
 * @date 2018-04-14
 */
public class Test {

    @Test
    public void test() {
        MultiValueMap<String, Object> bodyMap = new LinkedMultiValueMap<>();
        bodyMap.add("info", "info");
        // 图片资源
        bodyMap.add("photos", new FileSystemResource(new File("D:/photo/aa.png")));
        bodyMap.add("photos", new FileSystemResource(new File("D:/photo/bb.png")));

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.MULTIPART_FORM_DATA);
        HttpEntity<MultiValueMap<String, Object>> requestEntity = new HttpEntity<>(bodyMap, headers);

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.exchange("http://api.test.com", HttpMethod.POST, requestEntity, String.class);
        System.out.println(response.getStatusCode() + " " + response.getBody());

    }
}
```
