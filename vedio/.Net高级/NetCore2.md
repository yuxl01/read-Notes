# .NetCore(二)

### 常见应用
###### 一、配置文件的读取
  `利用Startup类中的configuration读取appsettings.json中的配置`
  ```.js
  {
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "option1": "value1_from_json",
  "option2": 2,
  "subsection": {
    "suboption1": "subvalue1_from_json"
  },
  "wizards": [
    {
      "Name": "Gandalf",
      "Age": "1000"
    },
    {
      "Name": "Harry",
      "Age": "17"
    }
  ],
  "AllowedHosts": "*"
}
```
  
```.cs
 public IConfiguration Configuration { get; }
 public Startup(IConfiguration configuration)
 {
     Configuration = configuration;
 }
 
 this.Configuration["Option1"]}"
 ```
  
