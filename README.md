# Introduction

I am a web development and I have written lots of notes in my daily study life, I decided to share with others by published on github.

I marked these notes in OneNote(a Microsoft application), it will take me some time to convert words to markdown, I can use some spare time to finish it.

I am a Chinese, so it is convenient to write in Chinese. Maybe I will write an English version someday if it has a lot of stars.

# preparation
 - **You need to install node environment to run gitbook**

    you can read the document in nodejs official website to learn how to preparation node environment
 - **Install gitbook**
    ```
    npm install gitbook-cli@2.1.2 --global
    ```
    tips: the latest version does't worked on my device, I have searched this problem on issue and stackoverflow, maybe there is something conflict in the latest source code. The version 2.1.2 worked.

# usage
```
git clone https://github.com/leonopteryx130/notes_docs.git
gitbook install
gitbook serve --port 4000
```
Type ```localhost:4000``` in the browser address, and then, you can read my documents