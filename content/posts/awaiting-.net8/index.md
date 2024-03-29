+++
title = '.NET Journey: Awaiting .NET 8'
date = 2023-09-23T12:46:30+04:00
draft = false
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
categories = ["General"]
keywords = [".NET evolution", "C# development", ".NET Framework history", ".NET 8 features", ".NET MAUI introduction", "Cross-platform .NET development", "Transformative journey of C# and .NET", "Milestones in .NET development", "Key features of .NET Framework 1.0", "Evolution of C# language", "Advantages of .NET 5 over previous versions", "Performance improvements in .NET 6/7", "Introduction to .NET MAUI for cross-platform GUI", "What to expect in .NET 8 release", "Native AOT for web applications in .NET 8", "Blazor and Entity Framework updates in .NET 8", "Common Language Runtime (CLR)", "Base Class Library (BCL)", "ASP.NET WebForms", "ADO.NET data access", "Windows Forms development", "Object-Oriented programming in C#", "Syntax simplicity in C#", "Garbage collection in C#", "Type-safety in C#", "Declarative programming with C# attributes", "Consolidation of .NET Core and .NET Framework in .NET 5", "Unified platform for different workloads in .NET 5", "Cross-platform support in .NET 5", "Performance improvements in .NET 6/7", "Cross-platform GUI development with .NET MAUI", "Native AOT compilation for web applications in .NET 8", "Blazor and Entity Framework updates in .NET 8"]
summary = "The Evolution of C# and .NET: From .NET Framework 1.0 to .NET 8 - A Journey of Innovation and Transformation!"
description = "Take a fun-filled rollercoaster ride through the history of C# and .NET, from their humble beginnings to the latest and greatest features in .NET 8. Join me as I reminisce about the old days, celebrate the changes along the way, and get hyped for what's coming next in the world of .NET development! 🚀"
coverImage="/images/dotnet.webp"
coverImageAltText="A purple background featuring white text that reads '.NET'"
+++

{{<img src="/images/dotnet.webp" align="center" alt="A purple background featuring white text that reads '.NET'" >}}<br>

While the tech world awaits for .NET 8, it’s an opportune moment to take a step back and reflect on the transformative journey of C# and .NET. Their evolution was not just a testament to Microsoft’s vision but also the dynamic and ever-changing demands of developers worldwide. This article offers a whirlwind tour of their metamorphosis, highlighting key milestones, and setting the stage for the exciting innovations .NET 8 promises to bring.

## How it all began

### .NET Framework 1.0

{{<img src="/images/dotnet_old.webp" alt="A white background featuring black text that reads 'Microsoft .NET'">}}

Released in 2002, **.NET Framework 1.0** was Microsoft's answer to a changing software development landscape, aiming to provide a unified environment for developing web services, desktop applications, and other software. The first release included many of the key features that are still being used today, even if they’ve changed as time went by, these were:

- **CLR:** Common Language Runtime
- **BCL:** Base Class Library
- **ASP.NET:** For web-based applications, introducing WebForms which allowed for event-driven web development
- **ADO.NET:** A set of classes for data access, offering disconnected data architecture and XML integration
- **Windows Forms:** For building rich desktop applications and even with the release of WPF or .NET MAUI they’re still being used
- **Support for multiple languages:** One of the main advantages was its ability to support multiple languages, ensuring interoperability

As you can see, even though .NET platform is constantly changing and evolving, the key features were a fundament for what we have today, which is being polished till this day. We’ve talked about the platform and now you might be wondering what did C# 1.0 had to offer back then?

### C# 1.0

Introduced alongside .NET Framework, C# 1.0 became one of the primary languages designed for the platform. Anders Hejlsberg, who also worked on Turbo Pascal and Borland Delphi, was a chief architect behind C#. As you may guess, C# 1.0 was not so rich on features as today, but in the same way as .NET Framework 1.0, it already had all the key concepts included that we’re still using to this day and they’re nowhere to go.

- **Object-Oriented:** Full support for classes and the three pillars of Object-Oriented programming (inheritance, polymorphism and encapsulation)
- **Syntax**, which was heavily influenced by Java and C++, but with a focus on simplicity and productivity, which was also related to the next key feature
- **Garbage collection:** Automatic memory management to avoid common programming pitfalls
- **Type-Safety:** C# is designed to be strongly typed to minimize runtime errors and enhance performance
- **Declarative programming:** Attributes in C# allow developers to add metadata to assemblies, classes etc., which could be then queried at runtime using reflection

## What has changed

{{<img src="/images/dotnet_5.webp" alt="A white background indicating new features coming with .NET 5">}}

Although **.NET Framework** and **C#** went a long way and there was a ton of features added year to year, the major changed happened in November, 2020, when **.NET 5** was released. You might be asking, what did it change for us, developers? Well, let’s see:

- **Consolidation of .NET Core and .NET Framework: .NET 5** was a successor to .NET Core 3.1, it brought an end to the separate **.NET Core** branding. It’s not here to continue what **.NET Framework** is, but to represent the path forward for the **.NET** platform as a whole.
- **Unified Platform for Different Workloads: .NET 5** allows developers to target various application types such as web, cloud, mobile, gaming, IoT and AI using a single **.NET** platform.
- **Cross-Platform Support: .NET 5** maintained and improved upon **.NET Core’s** commitment to cross-platform development, enabling developers to run their applications on Windows, Linux and MacOS.

All of this sounds pretty cool, although **.NET 5** didn’t bring us any GUI development libraries which were cross-platform and existing ones like **WinForms** or **WPF** remained Windows-only technologies.

### .NET 6/7

In terms of major changes, there were not so much features offered with **.NET 6/7** releases as in previous versions, but they both provided significant performance improvements and also, we finally have a technology for making GUI applications cross-platform, although, still not possible for Linux. What I’m talking about is - **.NET MAUI**, which is pretty good, but comes with some caveats.

- Yes, you have a cross-platform GUI technology, but Linux is still missing out.
- Yes, you have the ability for AOT compilation to run basically on bare metal, without .NET Runtime installed at all, but not for cross-platform apps, this is still a Windows-only feature.
- And last one, **.NET MAUI** is actually pretty good, but you have to write it’s styling and markup using **HTML & CSS**, which might be a plus for someone, although old **C#** developers might be more used to **WinForms** style or **XAML**.

## What to Expect

{{<img src="/images/dotnet_maui.webp" alt="A purple background featuring the white .NET MAUI logo" >}}

Even though **.NET 8** is set to release in **November, 2023**, we already can try it out! Yes, the **RC1** is already available and it has a lot to offer, including things like **Native AOT** for Web applications, **Blazor** improvements and **Entity Framework** updates, make sure to check out all the [benchmarks](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-8/) (really big performance improvements) and [RC1 notes.](https://devblogs.microsoft.com/dotnet/announcing-dotnet-8-rc1/) From the revolutionary advancements in Web application development to performance leaps, **.NET 8** promises a transformative experience for developers. So why wait? Dive into the **RC1** today and be among the first to feel the power of the future of **.NET**!

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read :)

If you enjoyed the article, you're welcome to join the [Discord](https://discord.gg/9SNyRghp) community, or [Telegram](https://t.me/anakidzedev) channel.
