---
layout: post
title:  "Docker built-in container support for .NET 7"
date:   2022-10-10 19:50:00 -0300
categories: dotnet csharp docker containerapps
---
On 14th September 2022, Microsoft announced .NET 7 Release Candidate 1, the first of two release candidates (RC) for .NET 7 that are supported in production.

The .NET 7 launch is scheduled for November 8-10, 2022 on the [.NET Conf 2022](https://www.dotnetconf.net/)! Until then we can try some of the new features and improvements of this new [.NET release](https://devblogs.microsoft.com/dotnet/announcing-dotnet-7-rc-1/#:~:text=Don't%20forget%20about%20.,NET%207%20release)!


Among several new and interesting features, one that I particularly liked and would like to highlight here is just the docker built-in container support for .NET 7.

This new .NET 7 resource, which is part of one of the new cloud native features, helps .NET to further consolidate itself as an excellent alternative for building cloud-native applications and achieving resiliency, scalability, efficiency and speed in your web apps.


#Let's get started

_For this release, you must have Docker and .NET 7.0.100-rc.1.22431.12 or higher installed. Additionally, only Linux-x64 containers are supported_.

Below we can see how simple it is to build a containerized ASP.NET application from scratch.

```
# 1st step - create a new project 
dotnet new mvc -n my-containerized-app

# 2nd step - move project to its directory
cd my-containerized-app

# 3rd step - add a reference to a (temporary) package that creates the container
dotnet add package Microsoft.NET.Build.Containers

# 4th step - publish your project for linux-x64
dotnet publish --os linux --arch x64 -p:PublishProfile=DefaultContainer

# 5th step - run your app using the new container
docker run -it --rm -p 5010:80 --name my-cloud-native-app my-containerized-app:1.0.0

```

If you are familiar with docker and the .NET CLI, or have already mastered them, you will probably have no difficulty understanding the above sequence of instructions. 

However, if you're a _beginner_, let's take a look at each statement, step by step.

In the ___first___ and ___second___ steps, we just created a new .NET application with the _ASP.NET Core Empty_ template and then moved to the new project directory.

```
$ dotnet new mvc -n my-containerized-app
$ cd my-containerized-app
```
- `dotnet new` : command to create a new project
- `mvc` : argument to set the .NET template project
- `-n` : option for the created output

In the ___third___ step, we added `Microsoft.NET.Build.Containers`, a [nuget package](https://www.nuget.org/packages/Microsoft.NET.Build.Containers) for publishing .NET applications natively as containers.

```
$ dotnet add package Microsoft.NET.Build.Containers
```

In our example we are using the [empty mvc template](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-6.0&tabs=visual-studio-code), but in your own project what you need to do is just add this package to it.

Taking a look at a _`my-containerized-app.csproj`_ file you can see the package there:
```
  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Build.Containers" Version="0.1.8" />
  </ItemGroup>
```


In the ___fourth___ step we compiled the application.
```
$ dotnet publish --os linux --arch x64 - p:PublishProfile=DefaultContainer
```

- `dotnet publish`: command to compile the app
- `--os linux` and `--arch`: options to specify the target operating system (OS) and specify the target architecture. We specified linux and x64, respectively. And it’s important to remember that only ___Linux-x64___ containers are supported
- `-p`: used for setting properties

If you type `docker images` in your console you could see that the image is already created:
```
REPOSITORY             TAG       IMAGE ID       CREATED         SIZE
my-containerized-app   1.0.0     b65f7ee7668a   7 seconds ago   220MB
```

Now, in the ___fifth___ and last step we finally run our container.
```
$ docker run -it -p 5010:80 --name my-cloud-native-app  my-containerized-app:1.0.0
```
- `docker run` : container process that runs is isolated in that it has its own file system, its own networking, and its own isolated process tree separate from the host
- `-it` : to run the container in interactive mode instead of detached mode, allowing us to execute commands while the container is in a running state
- `-p 5010:80` : definition of used ports. `<port that we will use on our local host>:<port used inside the container>` 
- `--name my-cloud-native-app` : to set a name for the container, preventing docker from generating a random name for it   
- `my-containerized-app:1.0.0` : setting the image that we want to use, in this example, `<name of our project>:<version defined by .NET>`

If all goes well we will have a result similar to this below.
```
warn: Microsoft.AspNetCore.DataProtection.Repositories.FileSystemXmlRepository[60]
      Storing keys in a directory '/root/.aspnet/DataProtection-Keys' that may not be persisted outside of the container. Protected data will be unavailable when container is destroyed.
warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
      No XML encryptor configured. Key {2b3bd45e-0cc8-4cad-a0b9-ce5593370e33} may be persisted to storage in unencrypted form.
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:80
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app
```

Then, just access the application in any browser:`http://localhost:5010/`:

![Application running](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k5r1x5nyfcfr32jq7zim.png)



#Conclusion

So, that 's it. We could see how simple this resource is and how _useful_ it is for our projects. Unlike virtual machines, containers can rapidly scale in and out and are essential to cloud-native applications, offering granular _scalability_, _portability_ and _efficient_ use of _resources_. 
