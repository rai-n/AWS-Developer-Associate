## EventBridge 
Allows you to connect sources and target as part of event system
E.g. There is an event sources which allows you to connect to event consumers
- Helps make things loosely coupled

![](https://i.imgur.com/VY5pcqo.png)

## Virtual ordering system
- creating order bus called orders

![](https://i.imgur.com/HANZuwz.png)

- creating rule for bus

![](https://i.imgur.com/BhA9nwd.png)

- any rule with matching source is not written to cloudwatch logs

![](https://i.imgur.com/6YSLM7j.png)

- using event generator to test

![](https://i.imgur.com/crhp5NE.png)

- checking on CloudWatch logs

![](https://i.imgur.com/UKCqJbt.png)

- creating event bridge for API destination

![](https://i.imgur.com/X1ne5tV.png)


- creating step function for processing eu orders

![](https://i.imgur.com/1N89Q2o.png)


### SNS Challenge 
> Process only orders from US locations (us-west or us-east) that are lab-supplies using a Amazon SNS target (Orders). Similar to the previous use case, but using SNS.

![](https://i.imgur.com/VhuRcPW.png)



### Schedule
![](https://i.imgur.com/4DhM3EQ.png)
![](https://i.imgur.com/Jh1avhN.png)


### Partner event source

![](https://i.imgur.com/dss8PPB.png)
![](https://i.imgur.com/DDbkgMG.png)
![](https://i.imgur.com/AHdJYUn.png)
![](https://i.imgur.com/neClivP.png)
![](https://i.imgur.com/YFfZ2fS.png)

#### AWS C9

![](https://i.imgur.com/QYvJhl6.png)


#### AWS SAM CLI 
- Creating resources for forecast service

![](https://i.imgur.com/scjUe2P.png)

- Provisioning resources

![](https://i.imgur.com/AlY21WB.png)
![](https://i.imgur.com/xzwvoRY.png)