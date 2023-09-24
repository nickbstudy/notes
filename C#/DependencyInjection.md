If you are using a WinForm or console app you need to first install the NuGet package Microsoft.Extensions.Hosting (3rd party alternatives are Autofac and Ninject).  

In Program.cs create a host builder interface, and register the services 