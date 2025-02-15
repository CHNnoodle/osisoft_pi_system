# Prerequisite
- PI ProcessBook v3.2.3.0 
- Anaconda v4.2.9

# Main function *getting data from PI Server*
Before you commit code, you need to specify server address and tag name. 
If you are not using Django framework, do not commit CoInitialize() / CoUninitialize().
```{.python}
import pythoncom
import win32com.client as win32
import numpy as np

pythoncom.CoInitialize()
server = win32.Dispatch('PISDK.PISDK.1').Servers(server_address)
pisdk = win32.gencache.EnsureModule('{0EE075CE-8C31-11D1-BD73-0060B0290178}',0, 1, 1,bForDemand = False)
tag = tag_name
point = server.PIPoints(tag).Data
n_samples = 250
data2 = pisdk.IPIData2(point)
results = data2.InterpolatedValues2('*-'+str(n_samples)+'h','*','1h',asynchStatus=None)
tmpValue =[]
tmpTime = []
i = 1 - n_samples
for v in results:
    try:
        s = str(v.Value)
        tmpValue.append(float(s))
        tmpTime.append(i)
    except ValueError:
        pass
    i = i + 1
tmpValue.pop()
tmpTime.pop()
result_set = np.array([tmpTime, tmpValue], dtype=np.float64)
pythoncom.CoUninitialize()
```

# Example Result
![Result](https://github.com/yoonsungkim87/osisoft_pi_system/blob/master/trend.png)
