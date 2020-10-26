# htaccess

ไฟล์ .htaccess คือไฟล์ที่ใช้สำหรับปรับ config ที่อยู่ใน Apache ที่มีชื่อว่า httpd.conf

**ทำไมต้องใช้ และใช้ยังไง ?**

เพราะเว็บแต่ละเว็บที่อยู่ใน Host หรือ Server เดียวกัน อาจมีความต้องการบางอย่างที่ไม่เหมือนกัน โดยปกติแล้วผู้ดูแล Server จะ Config ความต้องการหลักที่เป็นมาตรฐานที่คิดว่าผู้ใช้งาน หรือลูกค้าของเขาน่าจะใช้ ถ้าเรามีเว็บเดียวแล้วไปเช่า Host เขา เราอาจมีบางอย่างที่คิดต่างจากที่เขากำหนด แต่เราไม่ใช่ผู้ดูแล Server เราจึงไปแก้ config หลักใน httpd.conf ไม่ได้ สิ่งที่เราทำได้คือให้ใช้ไฟล์ .htaccess ในเว็บของเราเอง โดยสร้างไฟล์ชื่อ .htaccess ขึ้นมาแล้ว upload ไปไว้ที่ root ของเว็บเรา ...

**ความต้องการที่ต่างกัน ?**

สมมติเรามีหลายเว็บที่อยู่ที่ Host เดียวกัน สมมติว่า Web A ไม่ต้องการให้ผู้ใช้งาน Upload ไฟล์ แต่ Web B อาจต้องการให้ upload ไฟล์ได้ หรือ Web C อาจต้องการให้ Upload ไฟล์ได้แต่อยากกำหนดขนาดสูงสุดของไฟล์ที่ยอมให้ Upload ได้ เป็นต้น ... แบบนี้ .htaccess คือทางออก แต่ละเว็บก็จะมี .htaccess ของตัวเอง ที่วางอยู่ใน root ของเว็บตัวเอง

**ทำไมไม่ไปแก้ที่ httpd.conf ของ Apache ?**

ถ้าเราเป็นเจ้าของ Host เราเอง หรือเราใช้ Colo หรือ VPS แบบนี้ไม่มีปัญหา ปรับแต่ง แก้ไข httpd.conf ได้ตามสบายใจท่าน เพราะเราสามารถทำได้ทุกอย่าง แต่ถ้าเราเช่า Host คนอื่นเขาอยู่ ยิ่งเป็นแบบ Share Host คงไม่มีผู้ให้บริการยอมให้เราแก้ไข httpd.conf แน่ เพราะคนที่ให้เราเช่า ไม่ได้มีแต่เราที่เช่าเขา มีลูกค้าคนอื่นด้วย นั่นคือที่มาของการ config ค่าแบบกลาง ๆ เอา ไว้ ลูกค้ารายไหนอยากปรับอะไร ก็ไปทำที่ .htaccess ของตัวเองจบ

**ไม่จำเป็นต้องใช้ .htaccess ก็ได้ใช่ไหม ?**

เว็บใดที่ไม่มี .htaccess ของตัวเอง ก็จะต้องใช้ config หลัก httpd.conf ของ Apache คือเขากำหนดมายังไงก็ต้องใช้ไปอย่างนั้น ถ้าเว็บเราไม่ยุ่งยาก ซับซ้อน พอรับได้กับสิ่งที่ Server กำหนดมาให้ รวมทั้งเราไม่ต้องการ Force URL ให้เป็น https ไม่ต้องการ Force URL ให้มีหรือไม่มี www หรือเป็นช่วงที่ทำ demo ยังไม่ได้ production จริง ไม่จำเป็นต้องใช้ .htaccess ครับ

**เอา .htaccess มาแบ่งปันทำไม ?**

ไฟล์ทั้งหมดที่แบ่งปัน ผมทำเอง ใช้เอง ใช้จริง ลองแล้วทั้ง Project ที่ Run บน localhost, Share Host, Dedicated Server, Colo Server ที่แบ่งปันก็เห็นว่ามันใช้ได้ดี และเนื่องจากมันมีหลาย version กลัวลืม และอีกอย่างผมมีคอมหลายตัว เลยเก็บไว้ที่ Github ดีกว่า เวลาจะใช้จะได้เข้ามาเอาที่นี่ได้เลย และผมก็มักจะเอาตัวที่ไม่มี comment ไปใช้ เพราะจะได้เบา ตอน load ไฟล์ .htaccess

## ปัญหาหลัก (ที่ผมเคยเจอ)

- Apache Server Version ที่ต่างกัน
- PHP Version ที่ต่างกัน
- Enviroment ที่ต่างกัน
- Server ที่ต่างกัน
- Requirement ที่ต่างกัน
- PHP Setting ที่ต่างกัน

## ปัญหารอง (ที่ผมเคยเจอ)

- เขียน Comment ใน .htaccess เยอะไป เพราะกลัววันหลังจำไม่ได้ว่าอะไรคืออะไร ซึ่งเวลาไปใช้จริงหนักไป ไฟล์ใหญ่ไป ไม่เหมาะในการทำ url startup loading
- เขียนเอง งงเอง ถ้าจะเอา .htaccess ของ Project หนึ่ง ไปใช้กับอีก Project จะได้ไหม ถ้าไม่ได้เพราะบรรทัดไหน เนื่องจาก Apache Version, PHP Version, Server
- เคยมีไฟล์เดียว เวลาลูกค้าหรือตัวเราทำ Project ใหม่ ก็ต้องมานั่งดูว่าอะไรที่จะใช้ อะไรที่จะไม่ใช้ เสียเวลาทุกครั้งที่เริ่ม Project
- ผมมีปัญหาว่าบาง Project อยากให้ .htaccess ทำงานเฉพาะ Host หรือ Server จริง ไม่อยากให้ทำงานใน localhost ตอน Dev ผมก็ต้องแอบเอา .htaccess ไว้ใน folder อื่น ไม่กล้าไว้ใน root ของเครื่องตอน Dev เพราะถ้าเอาไว้มันจะทำให้ localhost ต้องทำตามที่ .htaccess เขียนด้วย ซึ่งอึดอัดมาก บางที่ก็อาจลืมได้เวลาไปไว้ที่อื่น หรือถ้าเปลี่ยนเป็น .htaccess.bak ก็ได้ แต่มันก็วุ่นวายอีก ต้องไป Rename เอา .bak ออกไปตอน upload ขึ้น Host จริง คือจะดีกว่ามากถ้ามันก็อยู่ใน root ที่เดิม และใช้ชื่อ .htaccess เดิม ทั้งตอน Dev และ Production

## สรุปเรื่องไฟล์ .htaccess ของผม

- `.htaccess` ของผม รองรับ Apache รุ่นเก่าตั้งแต่ 2.2.8 ที่มากับ AppServ จนถึงรุ่นล่าสุดปัจจุบัน เพราะผม update สิ่งที่ต้องเพิ่ม ต้องลบ ต้องแก้ให้เรียบร้อยแล้ว หมายความว่าเอาไปใช้ได้เลยตัวไหนก็ได้ **ไม่มีปัญหาเรื่อง Apache Version** แน่นอน รองรับทุก Host ทุก Server
- `.htaccess` ของผมมี Option เลือกได้ว่าจะให้มันทำงานกรณี Dev ใน localhost หรือจะไม่ให้มันทำงานตอน localhost ให้ทำเฉพาะ Host หรือ Server จริงอย่างเดียว
- `.htaccess` ของผม ได้เขียนทุกอย่างแยกออกเป็น 2 ชุด คือใช้สำหรับ Development และสำหรับ Production คือมีเวอร์ชัน **Comment-Version** และเวอร์ชัน **No-Comment-Version** เวลาใช้จริงผมจะเอาตัว No-Comment Version ไปใช้ จะได้ไฟล์เล็ก ไม่หนักไม่ถ่วงความเจริญ เวลาต้อง load ไฟล์ .htacess จะได้เบาสบายทุกครั้งที่ client เข้าชมเว็บ
- `.htaccess` ของผม ได้ทำ Option พวก Force ต่าง ๆ เอาไว้เป็นทางเลือก เช่น **[Force https]**, **[Force http]**, **[Force www]**, **[Force non www]**  เพราะแต่ละงานผมอาจจะใช้ Option ที่ต่างกันไป ตามความต้องการลูกค้า และตัวผมเอง
- `.htaccess` ของผม ได้เลือกเฉพาะสิ่งที่ดีที่สุดมาใช้ และทดสอบแล้วว่ามันใช้ได้จริง Inspire จาก **HTML5 Boilerplate, Stackoverflow** และอื่น ๆ
- `.htaccess` ของผม ใช้ **Security ที่ได้มาตรฐาน** รวมทั้งตัวป้องกัน **SQL Injection** ทีเขียนดักจากต้นทาง .htaccess เลย
- `.htaccess` ของผม **รองรับ PHP Multi Version** ผมได้ทำ Option ทางเลือกสำหรับคนที่ต้องการเอาไปใช้กับ PHP 5.2 ตัวเก่าที่ติดมากับ **AppServ** ตัวเก่าที่คนไทยชอบใช้, ผมทำตัวแยกเฉพาะสำหรับคนที่ใช้ **PHP 5** และผมทำตัวแยกเฉพาะสำหรับคนที่ใช้ **PHP 7** ... *เลือกที่ใช่ ใช้ที่ชอบ* เพราะลูกค้าผมรวมถึงงานของผมเองต่างก็ใช้ PHP version ต่างกันออกไป
- `.htaccess` ของผม ได้เตรียม Config PHP Setting ที่มี Option ทางเลือกสำหรับคนที่ต้องใช้ใน **Share Host** จริง ที่มีข้อจำกัดเรื่อง CPU, RAM, BW  รวมถึงคนที่ต้องการทำเฉพาะใน **localhost** ที่ไม่แบ่งทรัพยากรกับใคร ผมก็ได้ทำ Setting จัดเต็ม CPU, Memory, BW เพราะไม่ต้องแชร์กับใครมากมาย
- `.htaccess` ของผม ทำจริง ใช้จริง มีประสิทธิ์ภาพจริง ใครไม่ใช้ก็ไม่เป็นไร เพราะผมทำไว้ใช้ของผมเอง

## ข้อเสนอแนะ
- ถ้าชอบบอกได้ครับ แต่ถ้าไม่ชอบไม่ต้องบอกก็ได้ครับ
- ถ้าคิดต่าง เห็นต่าง หรือมีข้อเสนอแนะดี ๆ แนะนำได้ครับ
- ถ้าพบข้อผิดพลาด แจ้งได้ครับ
