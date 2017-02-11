# Setting up Selenium Server

`Protractor` is used for E2E test in Angular project. It is a Node.js program. It is required to set up with protractor configuration.
There are some examples for the configuration. However setting up Selenium related files is somehow complicated. 
[`Protractor`](www.protractor.org) site will be your first place to refer to as you study and set up environment. 
And also you may want to see [Selenium setup page](http://www.protractortest.org/#/server-setup). 

I want to share what I have experienced regarding Webdriver and Chrome driver issue. 

After installing `protractor` with `npm install`, I got this error.

```
Error: Could not find chromedriver at C:\Repos\FAR\node_modules\gulp-protractor\node_modules\protractor\selenium\chromedriver_2.21.exe
```

When I checked the directory, there was no chromedriver file, even if I installed protractor. I installed it and run `webdriver-manager update` like 

```
npm install protractor -g
webdriver-manager update
```

This is tricky. The command `webdriver-manager update` downloaded and installed the selenium related files into the global protractor folder.
So when I tried to run protractor with `gulp` locally it couldn't find the chrome driver. Many hours later I realized that I have to run it locally.

To sum up, if you want to run protractor tests with gulp, you should run `webdriver-manager update` locally.

### References

[Chrome Driver issue](https://github.com/teerapap/grunt-protractor-runner/issues/45)
