---
title: "Developing Pie for N7100 a journey through hell"
date: 2019-05-23
categories:
  - blog
tags:
  - N7100
---

## Oh my dear old Note II
Developing for Exynos is a pain anyway but when you start to talk about Exyons 4 in 2019 it's a diffrent story.
Getting sensors to work it's hell especially on N7100. The funny thing is that I9300 has almost the same sensors and because Victor Shilin made an awesome Los 16.0 and 15.1 of S3 you will think that you can just copy paste the code and job done. How nice would be...
Note II is using a stupid system to enable the sensors called **SSP** (God knows what's that suppose to mean)

```cpp
int ssp_sensor_enable(int sensor_type)
{
	char path_enable[PATH_MAX] = "/sys/class/sensors/ssp_sensor/enable";
	int value;
	int rc;

	ALOGD("%s(%d)", __func__, sensor_type);

	if (sensor_type < 0 || sensor_type >= SENSOR_FACTORY_MAX)
		return -EINVAL;

	value = sysfs_value_read(path_enable);
	if (value < 0)
		value = 0;

	value |= (1 << sensor_type);

	rc = sysfs_value_write(path_enable, value);
	if (rc < 0)
	{
		ALOGD("Check path_enable! ERROR! SENSOR: ", sensor_type);
		return -1;
	}

	return 0;
}
```

God how much I hate those sensors. Even after a long and awful battle with them they still don't work propelly. 
Why? No idea. They just hate me *pure hate*. Especially that stupid Proximity sensor

The problem with it is that it works no porblems with it. Then why it's not working in the ROMs you may ask... Well it's simple it just dosen't start but wait it's start's then the android calls it's deactivate function immediately. You can even trick it to work if you have my ROM installed you just simply need to 'start it twice' *Whatt?* Well just install 2 apps to test the sensors and put them in split screen you will see that it's working or be in a call then recive another call in waiting and it will work. **WTF** android. Meanwhile on I9300 side it work's without any problems and it's the same sensor *facepalm*...
---

Well let's see what else it's intersing on our side. Well hmmm **GPS**? 
Let's ignore that awesome GPS because it's reason to not work for us is .... well none. Same GPS chipset as on I9300 same everything nothing I've tried with original N7100 gps lib, I9300 gps lib, Manta, grouper everything that has **Broadcom BCM47511** No luck. Nobody can figure out what's happening with that as well. 
---

**N7100** is a very interesting phone and a big challenge. I still have hope that at some point I will figure out why it hates me. What I know for sure is that I wont give up. Especially now after all this time. 