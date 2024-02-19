+++
title = 'Stop Guessing, Start Measuring: Transform Your Code with BenchmarkDotnet!'
date = 2024-02-13
draft = false
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
tags = ["dotnet", "csharp", ".net", "benchmark", "backend"]
categories = ["Back End", "Guides"]
keywords = ["BenchmarkDotnet", ".NET application", "performance measurement", "migrating project", ".NET 5", ".NET 8", "benchmarking", "RuntimeMoniker", "targeting multiple versions", "[GlobalSetup]", "[Benchmark]", "setup methods", "helper methods", "identifying methods", "Baseline parameter", "creating benchmarks", "BenchmarkDotnet templates", "CLI", "Benchmark results", "SHA256", "MD5", "arithmetic mean", "confidence interval", "standard deviation", "10x engineer", "accuracy"]
+++

{{<img src="/images/benchmark_meme.webp" align="center" alt="Meme about benchmarking with Console.WriteLine">}} <br />

Imagine perfecting your .NET application only to struggle with performance measurement. BenchmarkDotnet resolves this, offering developers a streamlined solution for precise benchmarking. <!--more--> Are you in your way to migrating the project from .NET 5 to .NET 8? Is it really worth it? BenchmarkDotnet allows you to test your most time or memory consuming methods against multiple versions of .NET framework with ease and decide whether you really should do it.

## Demystifying BenchmarkDotnet

Let’s look at the first example you see, when you open up BenchmarkDotnet’s [website](https://benchmarkdotnet.org), or [Github](https://github.com/dotnet/BenchmarkDotNet) page.

```csharp
[SimpleJob(RuntimeMoniker.Net472, baseline: true)]
[SimpleJob(RuntimeMoniker.NetCoreApp30)]
[SimpleJob(RuntimeMoniker.NativeAot70)]
[SimpleJob(RuntimeMoniker.Mono)]
[RPlotExporter]
public class Md5VsSha256
{
    private SHA256 sha256 = SHA256.Create();
    private MD5 md5 = MD5.Create();
    private byte[] data;

    [Params(1000, 10000)]
    public int N;

    [GlobalSetup]
    public void Setup()
    {
        data = new byte[N];
        new Random(42).NextBytes(data);
    }

    [Benchmark]
    public byte[] Sha256() => sha256.ComputeHash(data);

    [Benchmark]
    public byte[] Md5() => md5.ComputeHash(data);
}
```

It’s not required for you to understand everything right now, BenchmarkDotnet has beautiful docs and you could dive straight into it after reading this article, but there are some key concepts that you should understand while using BenchmarkDotnet.

### 1. Targeting multiple versions

With BenchmarkDotnet you can easily test your code against multiple versions of .NET and all you have to do is using an attribute just as we saw in the example above.

```csharp
[SimpleJob(RuntimeMoniker.Net472, baseline: true)]
```

You could use as much of these as you want, you’re not really restricted, but it is mandatory that only one of them has `baseline: true` flag, it shouldn’t be more than that, but it shouldn’t be none as well, if you’re targeting multiple versions of .NET framework.

> Remember! It’s **crucial** to have all target versions of .NET installed on your system.

### 2. Moving setup away from benchmarks

- While making benchmarks, your code may require some additional data to work with and you typically don’t want to have the data generating process inside of your benchmarks, because then it counts as method execution time, which may not be true in all cases. BenchmarkDotnet solves this problem in an elegant way, you just have to make a method for data generation purposes and mark it as `[GlobalSetup]`.

```csharp
    [GlobalSetup]
    public void Setup()
    {
        data = new byte[N];
        new Random(42).NextBytes(data);
    }
```

- If your benchmarks have more specific needs, you could make a setup method for one benchmark only, which will be executed before the exact benchmark you specify, so it doesn’t slow down other ones.

```csharp
    [GlobalSetup(Target = nameof(Sha256))]
    public void Setup()
    {
        data = new byte[N];
        new Random(42).NextBytes(data);
    }
```

- If multiple benchmarks share the same data generation process, you don’t need separate setups for them, you can easily target **multiple** benchmarks as well.

```csharp
    [GlobalSetup(Target = new[] { nameof(Sha256), nameof(Md5) })]
    public void Setup()
    {
        data = new byte[N];
        new Random(42).NextBytes(data);
    }
```

### 3. Identifying methods to be benchmarked

Even though the code we're currently examining is relatively straightforward, benchmarks can often involve significant amounts of code, depending on the task at hand. With this in mind, you might have multiple setup methods, helper methods, etc, so it’s crucial to mark which methods need to be benchmarked exactly. The way BenchmarkDotnet does this is pretty straightforward, with the `[Benchmark]` attribute. One thing that the code above doesn’t tell you, this attribute can also have a `Baseline` parameter. Let’s say you’re refactoring some of the old code and you want to see how the new version compares to it, it’s pretty easy as well.

```csharp
[Benchmark(Baseline = true)]
public void OldVersionCode() {
    // Your old code...
}

[Benchmark]
public void NewVersionCode() {
	// Your new code...
}

public void SomeHelperMethod() {
	// Your helper method code...
}
```

Now with all the other data including mean time, etc, you will also get a new column called **Ratio** that indicates how your other benchmarks compare to your baseline benchmark. You can see more in the BenchmarkDotnet [docs](https://benchmarkdotnet.org/articles/samples/IntroBenchmarkBaseline.html) for this parameter.

## Creating your own benchmarks

Now that we’ve deep dived into BenchmarkDotnet, I think it’s time to run your first benchmark, you could use the same code provided by the docs, or write your own. For the sake of simplicity I’ll cover the one provided.

### Installing prerequisites

First thing you would want to do, to avoid writing all the code by yourself is installing BenchmarkDotnet templates which could be done via dotnet CLI.

`dotnet new install BenchmarkDotNet.Templates`

After it’s done, you could create your first benchmark from the CLI via

`dotnet new benchmark`

Or if you’re using a language different from C#, you could specify it as well

`dotnet new benchmark -lang F#`

`dotnet new benchmark -lang VB`

### Running your benchmarks

First thing you see after creating a benchmark project is a default `Program` and `Benchmarks` classes. `Program` is responsible for running benchmarks located at `Benchmarks` class. You could have as many benchmark classes as you want, but make sure to add all of them to `Program` if you’d like to run them as well. You could see that program contains pretty simple code

```csharp
 public class Program
    {
        public static void Main(string[] args)
        {
            var config = DefaultConfig.Instance;
            var summary = BenchmarkRunner.Run<Benchmarks>(config, args);

            // Use this to select benchmarks from the console:
            // var summaries = BenchmarkSwitcher.FromAssembly(typeof(Program).Assembly).Run(args, config);
        }
    }
```

Now if you have multiple benchmark classes and you only want to run several of them you could choose which ones to run with a slight change to your `Main` method.

```csharp
static void Main(string[] args) => BenchmarkSwitcher.FromAssembly(typeof(Program).Assembly).Run(args);
```

This way, BenchmarkDotnet will identify all the benchmarks your assembly contains and let you choose which ones to run.

### Benchmark results

I’ll be running slightly modified, in terms of targets, version of the same benchmark provided by the docs.

```csharp
  	[SimpleJob(runtimeMoniker: RuntimeMoniker.Net70, baseline: true)]
    [SimpleJob(runtimeMoniker: RuntimeMoniker.Net80)]
    public class Benchmarks
    {
        private SHA256 sha256 = SHA256.Create();
        private MD5 md5 = MD5.Create();
        private byte[] data;

        [Params(1000, 10000)]
        public int N;

        [GlobalSetup]
        public void Setup()
        {
            data = new byte[N];
            new Random(42).NextBytes(data);
        }

        [Benchmark]
        public byte[] Sha256() => sha256.ComputeHash(data);

        [Benchmark]
        public byte[] Md5() => md5.ComputeHash(data);
    }
```

If you were to run it, after it’s done, you’d see something similar to this

```csharp
| Method | Job      | Runtime  | N     | Mean      | Error     | StdDev    | Ratio | RatioSD |
|------- |--------- |--------- |------ |----------:|----------:|----------:|------:|--------:|
| Sha256 | .NET 7.0 | .NET 7.0 | 1000  |        NA |        NA |        NA |     ? |       ? |
| Sha256 | .NET 8.0 | .NET 8.0 | 1000  |  1.060 us | 0.0098 us | 0.0091 us |     ? |       ? |
|        |          |          |       |           |           |           |       |         |
| Md5    | .NET 7.0 | .NET 7.0 | 1000  |        NA |        NA |        NA |     ? |       ? |
| Md5    | .NET 8.0 | .NET 8.0 | 1000  |  1.486 us | 0.0084 us | 0.0078 us |     ? |       ? |
|        |          |          |       |           |           |           |       |         |
| Sha256 | .NET 7.0 | .NET 7.0 | 10000 |        NA |        NA |        NA |     ? |       ? |
| Sha256 | .NET 8.0 | .NET 8.0 | 10000 |  6.392 us | 0.0537 us | 0.0476 us |     ? |       ? |
|        |          |          |       |           |           |           |       |         |
| Md5    | .NET 7.0 | .NET 7.0 | 10000 |        NA |        NA |        NA |     ? |       ? |
| Md5    | .NET 8.0 | .NET 8.0 | 10000 | 11.898 us | 0.0236 us | 0.0209 us |     ? |       ? |
```

This table basically shows us comparison of these two algorithms for each version and for two different parameters in terms of `N`. BenchmarkDotnet makes sure you understand everything you see on this table, so after every benchmark you’ll be provided with legends grid, explaining what all of these means.

```csharp
// * Legends *
  N       : Value of the 'N' parameter
  Mean    : Arithmetic mean of all measurements
  Error   : Half of 99.9% confidence interval
  StdDev  : Standard deviation of all measurements
  Ratio   : Mean of the ratio distribution ([Current]/[Baseline])
  RatioSD : Standard deviation of the ratio distribution ([Current]/[Baseline])
  1 us    : 1 Microsecond (0.000001 sec)
```

## Conclusion

While BenchmarkDotnet will not supercharge your code to run faster or make you a 10x engineer, it’s nice to have in your toolbox. I remember making monstrosities out of stopwatches and `Console.WriteLine` just to compare the runtime speed of several methods, we’ve all been there and I’m pretty sure you can’t reach the same level of accuracy with those.

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read! :)
