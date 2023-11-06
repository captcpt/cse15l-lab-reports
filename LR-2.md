## Part I
### StringServer.java
```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

class StringHandler implements URLHandler {
    List<String> messages = new ArrayList<>();
    int sequence = 1;

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return "Add a message";
        } else {
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    String message = parameters[1];
                    String formattedMessage = sequence + ". " + message;
                    messages.add(formattedMessage);
                    sequence++;
                    return String.join("\n", messages);
                }
            }
        }
        return "Invalid request";
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new StringHandler());
    }
}
```

### Server.java
```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started!");
    }
}
```

#### Running `/add-message` on `StringServer`
![Image](https://github-production-user-asset-6210df.s3.amazonaws.com/84103589/277199191-fc915ff2-cf1c-40ba-b61e-29a75108ab45.png)
1. The Method(s) called was `handleRequest`.
2. The Relevant Argument for `handleRequest` was the `URI url` which in this case was represented by `/add-message?s=<string>`, and the Relevant Fields were `messages`, a list storing the strings and `sequence`, an integer tracking the number of the input string for each iteration.
3. `messages` list will gain new strings with every iteration; for example if at present `messages` was empty (`[]`), running `handleRequest` multiple times will result in (as seen in the screenshot), `["1. apple", "2. apple", "3. 10"]`, where the leading number/dot are represented by `sequence`, which increments by 1 per message added. So, overall to achieve this list, this is how it is updated:
    - `/add-message?s=apple` on empty `messages` and initial `sequence` (1) --> `messages` equals `["1. apple"]`, `sequence++` so `sequence` now equals 2;
    - `/add-message?s=apple` --> `messages` equals `["1. apple", "2. apple"]`, `sequence++` so `sequence` now equals 3;
    - `/add-message?s=10` --> `messages` equals `["1. apple", "2. apple", 3. "10"]`, `sequence++` so `sequence` now equals 4

![Image](https://github-production-user-asset-6210df.s3.amazonaws.com/84103589/277199262-2d307cfa-5523-4257-9cf2-09359829c446.png)

1. The Method(s) called was `handleRequest`.
2. The Relevant Argument for `handleRequest` was the `URI url` which in this case was represented by `/add-message?s=<string>`, and the Relevant Fields were `messages`, a list storing the strings and `sequence`, an integer tracking the number of the input string for each iteration.
3. `messages` list will gain new strings with every iteration; for example if at present `messages` was empty (`[]`), running `handleRequest` multiple times will result in (as seen in the screenshot), `["1. apple", "2. apple", "3. 10"...]`, where the leading number/dot are represented by `sequence`, which increments by 1 per message added. So, overall to achieve this list, this is how it is updated:
    - `/add-message?s=apple` on empty `messages` and initial `sequence` (1) --> `messages` equals `["1. apple"]`, `sequence++` so `sequence` now equals 2;
    - `/add-message?s=apple` --> `messages` equals `["1. apple", "2. apple"]`, `sequence++` so `sequence` now equals 3;
    - `/add-message?s=10` --> `messages` equals `["1. apple", "2. apple", 3. "10"]`, `sequence++` so `sequence` now equals 4
    - `/add-message?s=4309jriefk` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk"]`, `sequence++` so `sequence` now equals 5
    - `/add-message?s=+` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk", "5. +"]`, `sequence++` so `sequence` now equals 6
    - `/add-message?s=1` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk", "5. +", "6. 1"]`, `sequence++` so `sequence` now equals 7
    - `/add-message?s=[]` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk", "5. +", "6. 1", "7. []"]`, `sequence++` so `sequence` now equals 8
    - `/add-message?s=$$` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk", "5. +", "6. 1", "7. []", "8. $$"]`, `sequence++` so `sequence` now equals 9
    - `/add-message?s=messages***` --> `messages` equals `["1. apple", "2. apple", "3. 10", "4. 4309jriefk", "5. +", "6. 1", "7. []", "8. $$", "9. messages***"]`, `sequence++` so `sequence` now equals 10

## Part II
#### SSH Key Paths
- Private (On computer): `"C:/Users/chask/.ssh/id_rsa"`
<img width="189" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/f403e1f0-090e-4835-8f7f-e8f18331d138">
<img width="215" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/be73aa77-7fc9-46cb-b5fd-6e3f9ac54849">

- Public (On ieng6): `"~/.ssh/authorized_keys"`
<img width="161" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/26cc8d0a-d69d-4834-aa5a-7a4dd2a441ea">

#### Logging into ieng6 w/ SSH key
![Image](https://github-production-user-asset-6210df.s3.amazonaws.com/84103589/277200328-f33cd2db-c4e1-42dc-aebe-feac315432f2.png)

## Part III
I already have pretty good experience/exposure to SSH keys, command line, and VSCode (from other courses/work/personal projects), but I didn't really have as much experience with writing web servers with Java. Thus, I found from Weeks 2 and 3, writing and working with the `NumberServer.java` and creating the string and search servers was something new and interesting to utilize. I'm curious to see how I could possibly use this tool/implementation for other/more advanced purposes in the future.
