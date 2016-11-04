# how-to-download-facebook-avatar-in-asp.net

This code will show you how to download Facebook Avatar/Profile picture in ASP.NET.

###1. Create two classes as follow

```
    public class AvatarResponse
    {
        JsonProperty("data")
        public AvatarInfo Data { get; set; }
    }

    public class AvatarInfo
    {
        JsonProperty("height")
        public int Height { get; set; }
        JsonProperty("is_silhouett")
        public bool IsSilhouette { get; set; }
        JsonProperty("url")
        public string Url { get; set; }
        JsonProperty("width")
        public int Width { get; set; }
    }
```

###2. First request avatar url using FacebookId
```
         private string GetAvatarUrl(long facebookId)
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri("http://graph.facebook.com/");
            // Add an Accept header for JSON format.  
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            // List all Names.  
            HttpResponseMessage response = client.GetAsync(facebookId.ToString()+"/picture?width=200&height=200&redirect=false").Result;  // Blocking call!  
            if (response.IsSuccessStatusCode)
            {
                string jsonData = response.Content.ReadAsStringAsync().Result;
                AvatarResponse responseData = JsonConvert.DeserializeObject<AvatarResponse>(jsonData);
                if(responseData!=null && responseData.data!=null && !string.IsNullOrEmpty(responseData.data.url))
                {
                    return responseData.data.url;
                }
                else
                {
                    return null;
                }
            }
            else
            {
                return null;
            }
        }
```

###3. Then download the image from URL like this
```
           using (WebClient webClient = new WebClient())
            {
               //Other stuffs
    
                webClient.DownloadFile(url, newFilePath);
            }
```
