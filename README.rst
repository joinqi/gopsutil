gopsutil: psutil for golang
==============================

.. image:: https://drone.io/github.com/shirou/gopsutil/status.png
        :target: https://drone.io/github.com/shirou/gopsutil

.. image:: https://coveralls.io/repos/shirou/gopsutil/badge.png?branch=master
        :target: https://coveralls.io/r/shirou/gopsutil?branch=master


This is a port of psutil (http://pythonhosted.org/psutil/). The challenge is porting all 
psutil functions on some architectures...

Available Architectures
------------------------------------

- FreeBSD/amd64
- Linux/amd64
- Linux/arm (raspberry pi)
- Windows/amd64

(I do not have a darwin machine)


All works are implemented without cgo by porting c struct to golang struct.


Usage
---------

::

   import (
   	"fmt"

   	"github.com/shirou/gopsutil"
   )

   func main() {
   	v, _ := gopsutil.VirtualMemory()

   	// almost every return value is a struct
   	fmt.Printf("Total: %v, Free:%v, UsedPercent:%f%%\n", v.Total, v.Free, v.UsedPercent)

   	// convert to JSON. String() is also implemented
   	fmt.Println(v)
   }

The output is below.

::

  Total: 3179569152, Free:284233728, UsedPercent:84.508194%
  {"total":3179569152,"available":492572672,"used":2895335424,"usedPercent":84.50819439828305, (snip)}


Documentation
------------------------

see http://godoc.org/github.com/shirou/gopsutil


More Info
--------------------

Several methods have been added which are not present in psutil, but which provide useful information.

- HostInfo()  (linux)

  - Hostname
  - Uptime
  - Procs
  - OS                    (ex: "linux")
  - Platform              (ex: "ubuntu", "arch")
  - PlatformFamily        (ex: "debian")
  - PlatformVersion       (ex: "Ubuntu 13.10")
  - VirtualizationSystem  (ex: "LXC")
  - VirtualizationRole    (ex: "guest"/"host")

- CPUInfo()  (linux, freebsd)

  - CPU          (ex: 0, 1, ...)
  - VendorID     (ex: "GenuineIntel")
  - Family
  - Model
  - Stepping
  - PhysicalID
  - CoreID
  - Cores        (ex: 2)
  - ModelName    (ex: "Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz")
  - Mhz
  - CacheSize
  - Flags        (ex: "fpu vme de pse tsc msr pae mce cx8 ...")

- LoadAvg()  (linux, freebsd)

  - Load1
  - Load5
  - Load15

- GetDockerIDList() (linux)

  - container id list ([]string)

- CgroupCPU() (linux)

  - user
  - system

- CgroupMem() (linux)

  - various status

Some codes are ported from Ohai. many thanks.


Current Status
------------------

- done

  - cpu_times (linux, freebsd)
  - cpu_count (linux, freebsd, windows)
  - virtual_memory (linux, freebsd, windows)
  - swap_memory (linux, freebsd)
  - disk_partitions (linux, freebsd, windows)
  - disk_io_counters (linux)
  - disk_usage (linux, freebsd, windows)
  - net_io_counters (linux, freebsd, windows)
  - boot_time (linux, freebsd, windows(but little broken))
  - users (linux, freebsd)
  - pids (linux, freebsd)
  - pid_exists (linux, freebsd)
  - Process class

    - pid (linux, freebsd, windows)
    - ppid (linux, freebsd, windows)
    - name (linux)
    - cmdline (linux)
    - create_time (linux)
    - status (linux)
    - cwd (linux)
    - exe (linux, freebsd, windows)
    - uids (linux, freebsd)
    - gids (linux, freebsd)
    - terminal (linux, freebsd)
    - io_counters (linux)
    - nice (linux)
    - num_fds (linux)
    - num_ctx_switches (linux)
    - num_threads (linux, freebsd, windows)
    - cpu_times (linux)
    - memory_info (linux, freebsd)
    - memory_info_ex (linux)
    - memory_maps() (linux)
    - open_files (linux)
    - send_signal (linux, freebsd)
    - suspend (linux, freebsd)
    - resume (linux, freebsd)
    - terminate (linux, freebsd)
    - kill (linux, freebsd)

- not yet

  - cpu_percent
  - cpu_times_percent
  - net_connections
  - Process class

    - username
    - ionice
    - rlimit
    - num_handlers
    - threads
    - cpu_percent
    - cpu_affinity
    - memory_percent
    - children
    - connections
    - is_running


- future work

  - process_iter
  - wait_procs
  - Process class

    - parent (use ppid instead)
    - as_dict
    - wait


License
------------

New BSD License (same as psutil)


Related Works
-----------------------

I have been influenced by the following great works:

- psutil: http://pythonhosted.org/psutil/
- dstat: https://github.com/dagwieers/dstat
- gosiger: https://github.com/cloudfoundry/gosigar/
- goprocinfo: https://github.com/c9s/goprocinfo
- go-ps: https://github.com/mitchellh/go-ps
- ohai: https://github.com/opscode/ohai/


How to Contribute
---------------------------

1. Fork it
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create new Pull Request

My English is terrible, so documentation or correcting comments are also
welcome.
