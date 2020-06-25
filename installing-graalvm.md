# Installing GraalVM (easy way)

I've a Mac so installing GraalVM in my case wasn't just straightforward. So I found a easy and painless way to do it with SDK Man! 
This method is valid for Windows and Linux users too.



1. First install sdkman:
[https://sdkman.io/install](https://sdkman.io/install)

2. Then install the latest GraalVM version:

```
sdk list java
```

You should see something like this:
```
GraalVM        | >>> | 20.1.0.r11   | grl     |            | 20.1.0.r11-grl      
               |     | 20.1.0.r8    | grl     |            | 20.1.0.r8-grl       
               |     | 20.0.0.r11   | grl     |            | 20.0.0.r11-grl      
               |     | 20.0.0.r8    | grl     |            | 20.0.0.r8-grl       
               |     | 19.3.1.r11   | grl     |            | 19.3.1.r11-grl      
               |     | 19.3.1.r8    | grl     |            | 19.3.1.r8-grl
```

Then install the 20.1.0.r11 version:

```
sdk install java 20.1.0.r11
```

Next we need to select the GraalVM as default java version

```
sdk use java 20.1.0.r11
```

Then we need to install the native packages for GraalVM (in order to compile quarkus apps in a native way)

```
gu install native-image
```

> Note: when you install GraalVM, the command/app "gu" is installed too.



That's all. We are ready to compile Quarkus app in a native way with GraalVM doing:

```
mvn package -Pnative
```


Enjoy!