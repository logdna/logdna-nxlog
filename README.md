# LogDNA NXLog Windows Configuration

Use our NXLog configuration to get Windows logs into LogDNA securely, quickly, and reliably.

## How to Use

Follow the steps to use NXLog for forwarding your Windows logs to LogDNA:

### Install NXLog

* Install `NXLog Community Edition` from [here](https://nxlog.co/system/files/products/files/348/nxlog-ce-2.10.2150.msi), or
* Run `choco install -y nxlog` on `PowerShell` (make sure choco has been installed before running this command)

### Copy LogDNA NXLog Configuration

* Copy `nxlog.conf` to `$NXLOGDIR\conf\nxlog.conf` where `NXLOGDIR` is the directory where `nxlog` is installed in
* Modify `nxlog.conf` as described below:
	* Make sure to replace `CUSTOM_PORT` on [line 84](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L84) with a provisioned custom port which can be obtained in the [account-tailored add a log source instructions](https://app.logdna.com/pages/add-host)
	* Windows Event Logging is captured [here](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L61-L73):
		* Uncomment the lines to enable logging from the specified channels
		* Comment out the lines to disable logging from the specified channels
		* Add custom channels to enable logging from into the same `Query` block
	* Windows File Logging is capture [here](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L47-L59):
		* Update log directory where to stream logs from log files located in [line 48](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L48)
		* Update log files to stream logs from in [line 52](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L52)
	* All `input`, `processor`, and `output` channels are connected in [`route` block](https://github.com/answerbook/nxlogdna-agent/blob/master/nxlog.conf#L89-L91):
		* Comment out the whole block and remove from the `route` to disable logging from specific `input` channel
		* Add new `input` modules with unique names to be added to the `route` to enable logging from new sources

### Download LogDNA SSL Certificate Authority File

* Download `ld-root-ca.crt` from [here](https://assets.logdna.com/rootca/ld-root-ca.crt) to `$NXLOGDIR\cert\ca.pem`, or
* Run the following `PowerShell` script:
```bash
$url = "https://assets.logdna.com/rootca/ld-root-ca.crt"
$output = "$NXLOGDIR\cert\ca.pem"

(New-Object System.Net.WebClient).DownloadFile($url, $output)
```
	
### Start NXLog

* Run `nssm start nxlog` on `PowerShell` to start `NXLog`
* Run `nssm restart nxlog` on `PowerShell` to get new configurational changes applied
* Run `nssm stop nxlog` on `PowerShell` to stop `NXLog`

## Contributing

Contributions are always welcome. See the [contributing guide](/CONTRIBUTING.md) to learn how you can help. Build instructions for the agent are also in the guide.
