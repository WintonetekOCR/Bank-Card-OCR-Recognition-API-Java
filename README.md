# Bank Card OCR Recognition API (Java)

This repository provides a Java-based API for recognizing and extracting information from bank cards using Optical Character Recognition (OCR). The API can efficiently extract the card number, expiration date, and cardholder name, making it suitable for payment processing systems and digital wallets.

## Features
- Extracts key information from bank cards, including:
  - Card Number
  - Expiration Date
  - Cardholder Name
- Fast and accurate OCR using Tesseract OCR engine.
- Secure handling of sensitive card information.
- Simple RESTful API with JSON responses.
- Cross-platform support, can be deployed in any Java-based environment.
- Easily extendable for additional use cases like loyalty cards, membership cards, etc.

## Technologies Used
- **Java**: Core programming language.
- **Spring Boot**: Framework for building the REST API.
- **Tesseract OCR**: Optical Character Recognition engine.
- **OpenCV**: For image pre-processing (if needed).
- **Maven**: For dependency management.
- **Docker**: For containerization and easy deployment.

## How to Use
```
//dependency：okio-1.14.0,shiro-core-1.4.0,okhttp-3.11.0
import okhttp3.MultipartBody;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;
import okhttp3.Response;
import org.apache.shiro.crypto.hash.Md5Hash;
import java.io.IOException;
import java.time.Instant;

public class Test {

    public static void main(String[] args) throws IOException {

        String key = "M***********g";
        String secret = "3***********6";

        long timestamp = Instant.now().toEpochMilli();
        String token = new Md5Hash(key + "+" + timestamp + "+" + secret).toString();

        RequestBody body = new MultipartBody.Builder().setType(MultipartBody.FORM)
                .addFormDataPart("img", "/9j")
                .addFormDataPart("key", key)
                .addFormDataPart("token", token)
                .addFormDataPart("timestamp", String.valueOf(timestamp))
                .addFormDataPart("typeId", "17")
                .build();
        Request request = new Request.Builder()
                .url("https://flyocr.com/api/recogliu.do")
                .method("POST", body)
                .build();
        OkHttpClient client = new OkHttpClient();
        Response response = client.newCall(request).execute();
        System.out.println(response.body().string());
    }
}
```
