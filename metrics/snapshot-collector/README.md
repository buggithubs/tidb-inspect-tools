metrics-snapshot-collector
------

**This tool is used to capture screenshots of grafana**
### Preparation
- **Make sure grafana server has shared object file `libfontconfig.so.1`**
- debian/ubuntu:  
	- `apt-get install -y libfontconfig freetype-devel fontconfig-devel fontconfig`
- centos: 
	- `yum install -y fontconfig freetype freetype-devel fontconfig-devel libstdc++`
- **Make sure grafana server has fonts installed for English**
- to list the fonts support English: 
	- `fc-list :lang=en`
- if the output is empty, you will need to install at least one font that supports English, here is an example of install Google Fonts on Linux servers: 
	- `cd` 
	- `wget -O Open_Sans.zip  https://fonts.google.com/download?family=Open%20Sans` 
	- `unzip -d Open_Sans Open_Sans.zip` 
	- `sudo cp -rvf Open_Sans /usr/share/fonts` 
	- `sudo fc-cache -fv`
- centos: 
	- `sudo yum install -y open-sans-fonts`

### Build
- install Golang(1.8.3+)
- `make`

**The target executable binary file is bin/metrics-snapshot-collector**

### Usages
```
Usage of ./bin/metrics-snapshot-collector:
  -address string
    	grafana address (default "http://192.168.2.188:3000")
  -dashboard string
    	dashboard name
  -end string
    	end time,default is now (default "2017-12-04 10:20:34")
  -name string
    	panel name
  -password string
    	grafana password (default "admin")
  -renderurl string
    	render url
  -start string
    	start time, default is 3 days ago (default "2017-12-01 10:20:34")
  -timeout int
    	execute query timeout[second] (default 60)
  -user string
    	grafana user (default "admin")
```


### Examples:
- collect all panels
	- `./metrics-snapshot-collector -address "http://192.168.2.188:3000" -user "admin" -password="admin" -start "2017-12-01 10:20:34" -end "2017-12-04 10:20:34"`
- collect all panels of the `Test-Cluster-TiKV` dashboard
	- `./snapshot-collector -address "http://192.168.2.188:3000"  -user "admin" -password="admin" -dashboard "Test-Cluster-TiKV"`
- collect one panel by URL
	- `./metrics-snapshot-collector  -user "admin" -password="admin" -renderurl "http://192.168.2.188:3000/dashboard/db/test-cluster-disk-performance?panelId=11&fullscreen&orgId=1"`
- collect one panel by name
	- `./metrics-snapshot-collector -address "http://192.168.2.188:3000"  -user "admin" -password="admin" -name "Disk Latency"`
	
