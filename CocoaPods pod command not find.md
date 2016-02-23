# Cocoapods could not work on Mac OS X EI Capitan (10.11.*)

### Situation Description

- CocoaPods version : 0.39.0
- Mac OS version : EI Capitan (10.11.*)

After updating the Mac OS from 10.10.* to `EI Capitan`, actually my Mac OS is `10.11.3` now, the cocoa pods doesn't work. 

It will show the warning
		
	-bash: pod: command not found
	
just as I type any pod command in the terminate with enter, such as 

	$ pod search AFNetworking
	
 
### Cause
This is happening because Apple has enabled rootless on the new install.   
From 10.11.*, Apple seems to set the rootless as a default configuration. So we could not have the write permission in /usr/bin/pod, even though we use `sudo` which means `super user do`.

### Solution

#### solution 1
If we don't have the write permission, then we'd better to move the path to a writable place.   
This solution is a better one since it won't change the configuration of 
root. 

Type below command in the terminate.

	sudo gem install -n /usr/local/bin cocoapods
	
However, it doesn't work for me only run the command as above.

Thanks to STACKOVERFLOW!!!
http://stackoverflow.com/a/32883578/701926

If the command doesn't work for you either, try below,

		sudo chmod -R 755 /usr/local/bin
		
		
  
#### solution 2


If you type:

	sudo nvram boot-args="rootless=0"; sudo reboot
  
in terminal.app, your computer will reboot with it disabled.

Once that is done, type:

	sudo gem install cocoapods -V
  
the -V is for verbose and will spit out any errors if they happen.


And then, I hope that I could say CONGRATULATIONS to you ^O^

### Other Reference
http://stackoverflow.com/questions/30812777/cannot-install-cocoa-pods-after-uninstalling-results-in-error#comment52846432_30851030  

http://arstechnica.co.uk/apple/2015/09/os-x-10-11-el-capitan-the-ars-technica-review/8/#h1