# Project Smart Door Berbasis IoT 

**Name:**
1. FERIKHO FATIH AZHAR (1103202190)
2. MOHAMAD AQWAM FARID (1103201263)
3. KRESHNA PUTRA S.    (1103202215)
4. NURDIN              (1103204006)
5. ADAM AL-AZIZ OLII   (1103204045)

# Ringkasan Project
Project ini bertujuan untuk merancang sistem smart door atau pintu cerdas berbasis IoT. Sistem ini nantinya akan dapat mendeteksi apabila terjadi usaha pembobolan pintu yang tidak diinginkan, lalu mengirim sinyal yang dapat memicu alarm untuk berbunyi.

**Perkiraan waktu pengerjaan yang dibutuhkan**

Sekitar 3 jam


# Pemdahuluan
**Alasan memilih project:**
Kemajuan teknologi mendorong peningkatkan kualitas serta efektivitas segala aspek kehidupan manusia. Keamanan adalah salah satu aspek yang perlu untuk diperhatikan dan senantiasa diperbarui kualitasnya. Dengan sistem smart door yang kami buat, proses monitoring keamanan pintu dapat dilakukan dengan lebih mudah dan efisien serta lebih optimal dalam penggunaan sumber dayanya.

**Purposes of the project:**
Dapat memahami konsep IoT sederhana serta menerapkannya dengan cara merancang suatu alat, yakni Sistem Smart Door Berbasis IoT.

**Insights gained by doing the project:**
Dapat memahami dasar-dasar dalam perancangan alat IoT meliputi pembuatan kodingan, rangkaian elektronik, jaringan, serta database. 

# Alat-alat
Komponen | Fungsi | Jumlah | Harga Satuan
-| -| -| -
ESP8266 NodeMCU V3 | Mengumpulkan data dari sensor, melakukan transfer data, menggerakkan aktuator | 2 | Rp 55.000
Buzzer | Digunakan untuk alarm ketika pintu terbuka | 1 | Rp 1.800
MC-38 Sensor | Sensor magnetik yang dapat mengirimkan sinyal biner 0 atau 1 | 1 | Rp 9.000
Solenoid Lock | Sebagai aktuator untuk mengunci pintu | 1 | Rp 43.000
Relay 5V | Sebagai pengendali arus listrik rangkaian | 1 | Rp 5.000
Baterai 9V | Sebagai penyuplai daya pada rangkaian | 4 | Rp 10.000

![material](https://github.com/sweet-nightmare/IoT-Based-Smart-Door-Project/blob/main/AlatAlat.jpg)
Gambar 1 : Alat-alat yang digunakan


# Setup
**Setup IDE:**

Download dan Install PlatformIO pada VSCode untuk menyusun kode program.

Download dan install library yang diperlukan untuk project, antara lain :
1. PubSubClient
2. ESPNow
3. TaskScheduler

# Buat Rangkaian
The setup of the device and the wiring is shown in a circuit diagram in Figure 3. Note that i could not find the exact same microcontroller that i used but it has the same amount of pins on each side, so the diagram shows in what spot you to put the wires. I also could not find the exact soil moisture senor but this one has the exact same wiring.

![Screenshot 2022-07-03 153746](NodeDesign.png)

Figure 2: Wiring of the components

The EPS32 is put on in right side of the breadboard so the left side pins of the microcontroller can be used. It is powerd by a micro USB cable either connected to a power supply or to a computer. The Vin pin is connected to the breadboards power supply line to give power to the senors and the GND pin is conneced to the GND line. 

Both sensors have a GND pin that is also connected to the breadboards GND line VCC that is connected to the breadboards power supply line. Then each have one data transfer pin, since the capacitive soil moisture sensor is a analog senor then the data pin is wired to a ADC (Analogue to Digital Converter)  pin in this case GPIO36 and DHT11 is connected to GPIO14.

# Chosen platform
The platform i choose for this project is Adafruit since it is a free cloud service that seemed to fit my project with what it has to offer. It is able to easily visualize your data in real-time online in their dashboard where you can choose if you want to see it in a diagram, as a Gauge or any other form. It also offers the opportunity to analyse the data online in simple ways by going to the feeds.

![Screenshot 2022-07-03 161432](https://user-images.githubusercontent.com/108582271/177043837-c7ffd412-ce5b-40e6-ada5-160e3ae2873e.jpg)
Figure 3: The dashboard page on Adafruit

# The code
The first code part shown in figure 4 has first the necessary libaries that is needed for the project such as machine that is used for connecting to the microcontroller, dht that is used for getting the DHT11 sensor to work, network for connecting the device to wifi and mqtt for sending data over the internet.

Then there is variables that are used later in the code, configuration for network and adrafruit and then Setup of the sensors
![Screenshot 2022-07-04 175535](https://user-images.githubusercontent.com/108582271/177188324-6c395256-11cb-4a19-aa8a-82b2184e3fb5.jpg)
Figure 4: First code snippet

This is the main function of the code that tries to  measures data from the sensors, prints the values and then tries to send that data to adafruit and throws exceptions if anything goes wrong.
![Screenshot 2022-07-04 174224](https://user-images.githubusercontent.com/108582271/177186533-3771a6e4-dc13-4d42-8faf-777e1dd182b1.jpg)
Figure 5: Second code snippet

First we have the function restart_and_reconnect that restart the microcontroller and gets called if a exception gets called in the code that would make the code crash. Then do_connect is a function that connects the device to wife.

The code under that tries to set up a connection between the device and adafruit by using MQTT and calls restart_and_reconnect if something goes wrong. Then lastly we have the infinite loop that calls send_sensor_data() until a exception gets thrown and in that case calls restart_and_reconnect.

![Screenshot 2022-07-04 174313](https://user-images.githubusercontent.com/108582271/177186549-74b6d82a-0870-495f-9138-727c9018c406.jpg)
Figure 6: Third code snippet

# Transmitting the data / connectivity

I decided to send data once every 15 minutes since the purpose of this project is to know when i need to water my plant and since the moisture in the soil does not change that fast then 15 seemed like a good time beetween each measuring. 

Wifi was the wireless protocols used for this project because the micocontroller setup is in my home close to my router and do not need any protocol with longer range because of that. Wifi also has no recurring costs, low latency and less bandwidth restrictions so it seemed like to best options.

As for the transport protocols MQTT and webhooks is used in this project. MQTT is used for sending the data measured by the sensors to adafruit. It was choosen because it is a lightweight, energy-efficient and easy to use Transport protocol. Webhooks is also used for when the moisture level in the soil reaches under 30 percent to send a message to my discord that it is time to water my plant.

# Presenting the data
The dashboard on adafruit is set up by 3 feeds of data one for air moisture, one for soil moisture and one for temperature. Each of these 3 feed have 1 diagram showing how the value of each sensor has changed over the past 7 days. For the two moisture feeds a gauge was chosen to display att what percentage of moisture the sensors was at the last reading and for the temperature a text field shows what temeprature it was at the last reading.
![Screenshot 2022-07-03 191415](https://user-images.githubusercontent.com/108582271/177193430-7dc68de6-9abf-402a-b8de-eedfad6e255d.jpg)
Figure 7: The dashboard page on Adafruit

You can also go in to each feed where you can see a more detaild diagram over the data and get each value in a table where you can see the exact value and at what time it was sent.

![Screenshot 2022-07-03 191349](https://user-images.githubusercontent.com/108582271/177193444-15fbc08c-1af1-4815-84bb-8d8460aab52c.jpg)
Figure 8: The air moisture feed page on Adafruit

# Finalizing the design
After assembling the project this is result.
![20220704_204437](https://user-images.githubusercontent.com/108582271/177205477-455ef7d7-c685-43dc-b70d-454be5df1975.jpg)
Figure 8: Finalized project
![20220704_204443](https://user-images.githubusercontent.com/108582271/177205443-8add9629-6cae-42b0-b9d9-350867fc480f.jpg)
Figure 9: Capacitive soil moisture sensor
![20220704_204429](https://user-images.githubusercontent.com/108582271/177205457-6fb6f156-da45-4cb4-b2da-a7c26689adff.jpg)
Figure 10: Finalized project

**Final Thoughts**

I would say that the outcome of the project is satisfactory since i was able to accomplish what i had set out to do. Having prior experience with programming I found that the coding part of the project not that challenging except for getting the Capacitive soil moisture sensor to messure correct data which took quite some googeling to find a answer to and other than that everything whent on without any big struggels since the course had a lot of good turorials to follow.

