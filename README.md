# local-k8s-apps

# uptime
from(bucket: ${telegraf_bucket:doublequote})
|> range(start: v.timeRangeStart, stop: v.timeRangeStop)
|> filter(fn: (r) => r._measurement == "system")
|> filter(fn: (r) => r._field == "uptime")
|> filter(fn: (r) => r.host == "ditc-k8s-master")
|> last()
|> map(fn: (r) => ({ _value: float(v: r._value) }))

# cpu temp
from(bucket: ${telegraf_bucket:doublequote})
|> range(start: v.timeRangeStart, stop:v.timeRangeStop)
|> filter(fn: (r) => r._measurement == "cpu_temperature" )
|> map(fn: (r) => ({r with _value: r._value / 1000}))



[[outputs.influxdb_v2]]
urls = ["http://localhost:8086"]
token = "replace-in-bae64"
organization = "some-org"
bucket = "telegraf"
#In order to monitor both Network interfaces, eth0 and wlan0, uncomment, or add the next:
[[inputs.net]]
[[inputs.netstat]]
[[inputs.file]]
files = ["/sys/class/thermal/thermal_zone0/temp"]
name_override = "cpu_temperature"
data_format = "value"
data_type = "integer"

[[inputs.cpu]]
[[inputs.disk]]
[[inputs.diskio]]
[[inputs.kernel]]
[[inputs.mem]]
[[inputs.processes]]
[[inputs.swap]]
[[inputs.system]]

~


5 books for Software Engineers

1. Clean Architecture (Martin)
2. Building Microservices (Newman)
3. Unit Testing (Khorikov)
4. Domain Driven Design (Evans)
5. Head First Design Patterns (Freeman & Robson)