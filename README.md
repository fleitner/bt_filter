

Backtrace Filter
================


What is it?
-----------

It's a tool to easy the task of reviewing all backtraces in a vmcore, so
instead of looking at several similar backtraces, this python script
will group similar ones listing the PIDs.


## What are the system requirements?

You need to have python installed.


How to use it?
--------------

It's two step process:

1.  Dump to a file all the backtraces from the vmcore using the crash tool.

    ```
    crash> foreach bt > bt.all
    ```

2. On another shell, run the backtrace filter script:

   ```
   ./bt_filter.py bt.all
   ```

Output example
--------------

Instead of looking at the samples like the ones below repeated
more 40 times:


```
	PID: 1      TASK: dfe00aa0  CPU: 1   COMMAND: "init"
	 #0 [dfe01afc] schedule at c06076a4
	 #1 [dfe01b6c] schedule_timeout at c0607dea
	 #2 [dfe01b90] do_select at c04801db
	 #3 [dfe01e34] core_sys_select at c04804de
	 #4 [dfe01f74] sys_select at c0480aa5
	 #5 [dfe01fb8] system_call at c0404ef8
	    EAX: 0000008e  EBX: 0000000b  ECX: bfd0b310  EDX: 00000000 
	    DS:  007b      ESI: 00000000  ES:  007b      EDI: bfd0b440
	    SS:  007b      ESP: bfd0b2dc  EBP: bfd0b5d8
	    CS:  0073      EIP: 00a17402  ERR: 0000008e  EFLAGS: 00000246 

	PID: 557    TASK: e386b550  CPU: 13  COMMAND: "smbd"
	 #0 [e8719afc] schedule at c06076a4
	 #1 [e8719b6c] schedule_timeout at c0607dea
	 #2 [e8719b90] do_select at c04801db
	 #3 [e8719e34] core_sys_select at c04804de
	 #4 [e8719f74] sys_select at c0480aa5
	 #5 [e8719fb8] system_call at c0404ef8
	    EAX: 0000008e  EBX: 00000019  ECX: bfc151a4  EDX: 00000000 
	    DS:  007b      ESI: 00000000  ES:  007b      EDI: bfc1522c
	    SS:  007b      ESP: bfc1502c  EBP: bfc15108
	    CS:  0073      EIP: 00865402  ERR: 0000008e  EFLAGS: 00200246 
```

You will see this instead:

```
	Backtrace:
	#0  schedule at c06076a4
	#1  schedule_timeout at c0607dea
	#2  do_select at c04801db
	#3  core_sys_select at c04804de
	#4  sys_select at c0480aa5
	#5  system_call at c0404ef8
	PID List:
	     1   557  1269  1632  1721  2001  2617  4041  4053  4064
	  4334  4371  5607  5720  5984  6504  6553  7007  7165  8691
	  9213 11562 13354 13417 14196 16557 17132 17568 17573 17642
	 21435 21941 22107 22552 22556 22557 24497 26549 26598 26848
	 29025 30523
	Total of 42 PIDs
```

The output says that those 42 PIDs listed above share the same
backtrace. A lot easier, isn't? :)

Feedback, suggestions or patches are welcome.

Contact: Flavio Leitner <fbl at redhat dot com>


