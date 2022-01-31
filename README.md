## Labs For ECE 4960

You can use the [editor on GitHub](https://github.com/CupertinoVanguard/Webpage/edit/main/README.md) to maintain and preview the content for your website in Markdown files.


### Lab 1

Write Up: 
This lab involved setting up the artemis and experimenting with the Arduino IDE. The first example experimented with in this lab involved using `digitalWrite()` to blink the LED onboard by driving it high for half a second then driving it low for yet another half a second. The demonstration of this is included in link [1]. 

  The second task involved experimenting with the serial monitor. The taks involved (with code already provided) having the serial monitor output what it reads in as input. In such a fashion, it essentially _echoes_ the input. The video of the result is included in link [2]. 
  
  The next component involved experimenting with analog reading and A-to-D conversion. By utilizing the selected ADC pin that can sense temperature change and incoming analog voltage change, the analog value is stored using an integer. The analog value resolution is preset to be 16 bits on both read and write. The analog value is then written to the LED-BUILT_IN Pin using analogWrite(). Since the write occurs to a digital pin, PWM conversion occurs. The value read by the analogPin is divided by the maximal value from 16 bits to derive the duty cycle. This leads to a LED intensity change when additional rise in value is detected on the ADC pin specified. 
  
  The microphone task involved utilizing the PDM library to read and process the read from the microphone onboard. The PDM vallue is read into a buffer and processed using the FFT ibarary in the printLoudest method. Additionally, for the extra task, the print Loudest method is made to return the float value calculated. Though the assignemnt says to turn the LED on when the frequency is the whistling range, the range included is in the lower frequency range due to my inability to whistle. A screenshot of the amended code is included below: 

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/CupertinoVanguard/Webpage/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or 

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
