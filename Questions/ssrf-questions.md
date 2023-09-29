# Questions About Server-Side Request Forgery

SSRF - Force a vulnerable machine to make an HTTP request the owner did not intend

- WHY are you executing the attack?
    - Forge HTTP requests to internal services or resources to extract sensitive information.
    - Exploit SSRF to scan and enumerate the internal network or discover hidden services.
    - Manipulate SSRF to bypass security controls and access restricted data or functionality.
    - Redirect internal requests to external malicious servers, facilitating data exfiltration.
    - Craft SSRF payloads to perform unauthorized actions on behalf of legitimate users.
    - Redirect requests to external resources to abuse application functionality or cause disruption.
    - Make malicious HTTP Requests on behalf of the organization to damage their reputation and ultimately lead to financial loss.

- WHO is the victim?
    - The organization hosting the infrastructure

- What Technology are you exploiting??
    - Server-Side Code in Application (axios, HttpURLConnection, HttpClient, cURL, etc.)
    - Command Injection on Server (curl, wget, etc.)
    - CVE in infrastructure
    - Misconfigured cloud service

- WHEN will you execute the attack?
    - If Targeted: When the target service is up
    - If Enumerating: Enum will be noisy, best to run the attack at night?  Or better during the day with more noise?

- WHERE can I execute the attack?
    - Anywhere w/ Internet Connection

- HOW will you deliver the payload?
    - HTTP Request
    - User-controlled input (param, header, cookie, anything)


## Examples of Vulnerable Code

#### JavaScript

```
const axios = require('axios');

const url = req.query.url; // This URL should come from user input (e.g., a query parameter)

axios.get(url)
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error(error);
  });

```

#### Java

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class SSRFExample {
    public static void main(String[] args) {
        String inputURL = args[0];
        try {
            URL url = new URL(inputURL);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            System.out.println(response.toString());
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### C#/.NET

```
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        string inputURL = args[0]; // Assume inputURL comes from user input

        using (HttpClient client = new HttpClient())
        {
            try
            {
                HttpResponseMessage response = await client.GetAsync(inputURL);
                response.EnsureSuccessStatusCode();
                string responseBody = await response.Content.ReadAsStringAsync();
                Console.WriteLine(responseBody);
            }
            catch (HttpRequestException e)
            {
                Console.WriteLine($"Request error: {e.Message}");
            }
        }
    }
}

```

#### PHP
```
<?php
$inputURL = $_GET['url']; // Assume inputURL comes from user input

$ch = curl_init($inputURL);

curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);

if ($response === false) {
    echo 'cURL error: ' . curl_error($ch);
} else {
    echo $response;
}

curl_close($ch);
?>

```
