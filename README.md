# FASTDDS & ROS2 ~ Publisher/Subscriber



## Communication FASTDDS & ROS2

### Presequisites
- ROS2
- FASTDDS

### Setup DDS Environment
Copy **String.idl** file from ros directory to your workspace:
> cp /opt/ros/\<versione\>/share/std_msg_ms/msg/String.idl /home/\<workspace_directory\>

**Warning!!** Edit String.idl file if you want to call "String" your Struct. You must use an escape character "_String".

In the workspace directory, generate DDS files:
> fastrtpsgen -example CMake String.idl

Edit **StringPubSubTypes.cxx** file at line 37 in the **setName** method:
```
setName("std_msgs::msg::**dds_**::String<strong>_</strong>");
```

Edit **StringPublisher.cxx** or **StringSubscriber.cxx** edit the topic name with leading "rt/" (line 59):

```
..topicName = "rt/TopicName";
```

Make a directory called "build" and compile code, generating the executable file:

> mkdir build && cd build
> cmake ..
> make


### Setup ROS2 Environment

Follow the tutorial at https://index.ros.org/doc/ros2/Tutorials/Writing-A-Simple-Cpp-Publisher-And-Subscriber/
Do not run the executable yet.

Then Edit **publisher_member_function.cpp**(line 34):

```
publisher_ = this->create_publisher<std_msg::msg::String>("TopicName", 10);
```
In \<workspace>/cpp_pubsub, make a directory called "build" and compile code, generating executable the file:
<style> </style>
> mkdir build && cd build
> cmake ..
> make

### Execute

Open two different terminal. 
In the first terminal, go to the FASTDDS Environment and run:
> ./String subscriber

In the other terminal, go to cpp_pubsub/build and run:
> ./talker

Now you should see the two terminals communicating. 

### Edit message sent

If you want to see the message exchange, edit **StringSubscriber** in the FASTDDS environment and change your code in order to print the received messages(line 95):
```
std::cout << "Received: " << st.data().c_str() << std:endl; 
```

Now you should see the messages being printed in the subscriber console too.

Authors:
 - Caglioni Brian
 - Youssef Zraiba
