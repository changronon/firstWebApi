//學習來源 https://blog.miniasp.com/post/2020/09/09/Create-Controller-and-Views-with-dotnet-aspnet-codegenerator

建立 Web API 專案


1.建立專案


dotnet new webapi -n firstWebApi


2.進入專案


cd .\firstWebApi\


3.安裝 dotnet-ignore


dotnet tool update -g dotnet-ignore


4.加入 .gitignore 檔案


dotnet ignore get -n visualstudio


5.將專案加入 Git 版控


git init


git add .


git commit -m "Initial commit"


6.在資料庫中建立資料庫


7.安裝 dotnet-ef


dotnet tool update -g dotnet-ef


8.安裝 Entity Framework Core 相關套件


dotnet add package --version="5.0" Microsoft.EntityFrameworkCore.SqlServer


dotnet add package --version="5.0" Microsoft.EntityFrameworkCore.Design


dotnet add package --version="5.0" Microsoft.EntityFrameworkCore.Tools


dotnet add package --version="5.0" Microsoft.EntityFrameworkCore.Sqlite


dotnet add package --version="5.0" Microsoft.EntityFrameworkCore.InMemory


9.建立新版本


git add .


git commit -m "Add EFCore NuGet packages"


10.透過 dotnet-ef 快速建立 EFCore 模型類別與資料內容類別


dotnet ef dbcontext scaffold "Server=DESKTOP-T10T3TN\SQLEXPRESS;Database=ContosoUniversity;Trusted_Connection=True;MultipleActiveResultSets=true" Microsoft.EntityFrameworkCore.SqlServer -o Models


dotnet build


11.透過 dotnet-ef 工具產生程式碼的時候，會順便將「連接字串」一定產生在 ContosoUniversityContext.cs 原始碼中，需程式碼移除。


"ContosoUniversityContext.cs" move #warning


12.建立新版本


git add .


git commit -m "Create EFCore models and dbcontext classes using dotnet-ef"


13.調整 ASP․NET Core 的 Startup 類別並對 ContosoUniversityContext 設定 DI


"Startup.cs" add DI


▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼


public void ConfigureServices(IServiceCollection services)


{


    services.AddDbContext<ContosoUniversityContext>(options =>
    
    
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
        
        
    services.AddControllers();
    
    
}


▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲


14.調整 ASP․NET Core 的 appsettings.json 加入 DefaultConnection 連接字串


"appsettings.json" add


▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼


{


  "ConnectionStrings": {
  
  
      "DefaultConnection": "Server=DESKTOP-T10T3TN\\SQLEXPRESS;Initial Catalog=ContosoUniversity;Integrated Security=True"
      
      
  },
  
  
  "Logging": {
  
  
    "LogLevel": {
    
    
      "Default": "Information",
      
      
      "Microsoft": "Warning",
      
      
      "Microsoft.Hosting.Lifetime": "Information"
      
      
    }
    
    
  },
  
  
  "AllowedHosts": "*"
  
  
}


▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲


15.建立新版本


git add .


git commit -m "Add DI for ContosoUniversityContext and configure ConnectionStrings"


dotnet build



透過現有的 Models 快速建立 API Controllers 


1.需要額外安裝 Microsoft.VisualStudio.Web.CodeGeneration.Design 套件


dotnet add package --version="5.0" Microsoft.VisualStudio.Web.CodeGeneration.Design


2.執行 dotnet aspnet-codegenerator，可以看到 Available generators 的資訊，就代表環境已經準備好。


dotnet aspnet-codegenerator


3.建立新版本


git add .


git commit -m "dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design"


4.快速建立 Controller （沒有 Views 頁面）


dotnet aspnet-codegenerator controller -name CoursesController -async -api -m Course -dc ContosoUniversityContext -outDir Controllers
