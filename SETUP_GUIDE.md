# 📚 คู่มือการตั้งค่า Lab 03: LINE Gold Price Bot

## 📋 สารบัญ

1. [สร้าง LINE Messaging API Channel](#1-สร้าง-line-messaging-api-channel)
2. [ติดตั้งและตั้งค่า ngrok](#2-ติดตั้งและตั้งค่า-ngrok)
3. [รัน n8n](#3-รัน-n8n)
4. [ตั้งค่า LINE Webhook](#4-ตั้งค่า-line-webhook)
5. [ทดสอบระบบ](#5-ทดสอบระบบ)

---
d
## 1. สร้าง LINE Messaging API Channel

⚠️ **อัพเดท 2026:** ตอนนี้ LINE ไม่อนุญาตให้สร้าง Messaging API channel โดยตรงจาก LINE Developers Console แล้ว ต้อง**สร้าง LINE Official Account ก่อน** แล้วค่อย**เปิดใช้งาน Messaging API**

### 1.1 เข้าสู่ LINE Developers Console

1. ไปที่ [https://developers.line.biz/console/](https://developers.line.biz/console/)
2. คลิก **Log in with LINE account**
3. Login ด้วย LINE Account ของคุณ

### 1.2 สร้าง Provider

> Provider คือกลุ่มของ Channels ที่จัดการโดยบุคคล/องค์กรเดียวกัน

1. ถ้ายังไม่มี Provider:
   - คลิก **"Create a new provider"** หรือ **"Create"**
   - ใส่ชื่อ Provider เช่น `My LINE Bots` หรือ `StudentLab`
   - คลิก **"Create"**
2. ถ้ามี Provider อยู่แล้ว สามารถใช้ที่มีอยู่ได้เลย

### 1.3 สร้าง LINE Official Account

1. เลือก Provider ที่สร้างไว้
2. คลิก **"Create a Messaging API channel"**
3. คุณจะเห็นข้อความว่า:
   ```
   It's no longer possible to create Messaging API channels directly
   from the LINE Developers Console.

   To create a Messaging API channel, create a LINE Official Account...
   ```
4. คลิกปุ่มสีเขียว **"Create a LINE Official Account"**
   - (จะพาไปยังหน้าภายนอก: https://entry.line.biz/)

### 1.4 กรอกข้อมูล LINE Official Account

ในหน้าสร้าง LINE Official Account (ฟอร์มภาษาไทย) จะมี 3 ขั้นตอน:
1. ลงทะเบียนข้อมูลบริษัท/ร้าน
2. ตรวจสอบข้อมูลการสมัคร
3. เสร็จสิ้นการสมัคร

#### ขั้นตอนที่ 1: ลงทะเบียนข้อมูลบริษัท/ร้าน

**ข้อมูลบุคคล** (กรอกอัตโนมัติจากบัญชี LINE)
- ชื่อผู้ใช้: [ชื่อจากบัญชี LINE ของคุณ]
- ประเทศที่ใช้บริการ: ไทย

**ข้อมูลบัญชี** (ต้องกรอกเอง)

| ฟิลด์ | วิธีกรอก | ตัวอย่าง |
|------|---------|----------|
| **ชื่อบัญชี** | พิมพ์ชื่อ Bot (สูงสุด 20 ตัวอักษร) | `Gold Price Bot` หรือ `บอทราคาทอง` |
| **ยี่นตล** | เว้นว่างได้ (Optional) | - |
| **ประเภทธุรกิจ & ธุรกิจ** | คลิก dropdown → เลือก | `การเงินและประกันภัย` |
| **ชื่อบริษัท/ธุรกิจ** | เว้นว่างได้สำหรับ Lab | `Student Lab` |
| **ประเภทธุรกิจ** (dropdown 1) | เลือกหมวดหมู่หลัก | `การเงินและประกันภัย` |
| **ประเภทธุรกิจ** (dropdown 2) | เลือกหมวดหมู่ย่อย | `บริการทางการเงินอื่นๆ` |

**เลื่อนลงด้านล่างเพื่อกรอกข้อมูลเพิ่มเติม:**

| ฟิลด์ | วิธีกรอก |
|------|---------|
| **อีเมล** | กรอก email ของคุณ เช่น `your-email@example.com` |

**ข้อกำหนด:**
- ✅ ติ๊กถูก **"ฉันได้อ่านและยอมรับข้อกำหนดในการให้บริการ LINE Official Account"**

คลิก **"ต่อไป"** หรือ **"Next"**

#### ขั้นตอนที่ 2: ตรวจสอบข้อมูลการสมัคร

- ตรวจสอบข้อมูลที่กรอกว่าถูกต้อง
- คลิก **"ยืนยัน"** หรือ **"Confirm"**

#### ขั้นตอนที่ 3: เสร็จสิ้นการสมัคร

- คุณจะได้รับข้อความแจ้งว่าสร้าง LINE Official Account สำเร็จ
- ระบบจะพาคุณไปยัง **LINE Official Account Manager**

### 1.5 เปิดใช้งาน Messaging API

หลังจากสร้าง LINE Official Account เสร็จ ระบบจะพาคุณไปยัง **LINE Official Account Manager**

คุณจะเห็นหน้า Home ของ Official Account พร้อมชื่อบัญชีและ LINE ID (@xxx)

#### ขั้นตอนการเปิดใช้งาน Messaging API:

1. **คลิก ⚙️ Settings** ที่มุมขวาบน (ข้างๆ ชื่อผู้ใช้)

2. ในหน้า Settings จะมีแท็บหลายอัน:
   - Account settings
   - **Messaging API** ← คลิกแท็บนี้
   - Response settings
   - ฯลฯ

3. เลื่อนลงในหน้า **Messaging API** จนเจอส่วน **"Messaging API"**

4. คลิกปุ่ม **"Enable Messaging API"** (สีเขียว) หรือ **"Link Messaging API"**

5. จะมี popup ขึ้นมาให้เลือก Provider:
   - เลือก **Provider** ที่คุณสร้างไว้ใน LINE Developers Console
   - คลิก **"OK"** หรือ **"Confirm"**

6. รอสักครู่ ระบบจะสร้าง **Messaging API channel** ให้อัตโนมัติ

7. หน้าจอจะ refresh และคุณจะเห็นส่วน Messaging API settings เพิ่มเติม

**✅ สำเร็จ!** ตอนนี้คุณมี Messaging API channel แล้ว

### 1.6 ดึง Channel Access Token

1. หลังจากเปิดใช้ Messaging API แล้ว กลับไปที่ [LINE Developers Console](https://developers.line.biz/console/)
2. เลือก **Provider** และ **Channel** ที่เพิ่งสร้าง
3. ไปที่แท็บ **"Messaging API"** (บนสุด)
4. เลื่อนลงไปหาส่วน **"Channel access token (long-lived)"**
5. คลิก **"Issue"** (ถ้ายังไม่เคยสร้าง) หรือ **"Reissue"** (ถ้าสร้างแล้ว)
6. คุณจะเห็น Token ยาวๆ เช่น:
   ```
   eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0...
   ```
7. **คลิก "Copy"** เพื่อคัดลอก Token
8. **เก็บ Token ไว้ในที่ปลอดภัย** (จะต้องใช้ใน n8n ในขั้นตอนต่อไป)

```
⚠️ สำคัญมาก: อย่าเผยแพร่ Token นี้ในที่สาธารณะ!
   Token นี้เป็นรหัสลับที่ให้สิทธิ์ในการส่งข้อความจาก Bot
```

### 1.7 ตั้งค่า Response settings (สำคัญมาก!)

หลังจากตั้งค่า Messaging API และกรอก Webhook URL แล้ว ต้องตั้งค่า Response settings

#### ขั้นตอน:

1. ยังอยู่ในหน้า **LINE Official Account Manager** (Settings)
2. คลิกเมนู **"Response settings"** ที่ด้านซ้าย
3. คุณจะเห็นหน้า **"Toggle responses"** พร้อมสวิตช์หลายตัว

#### ปรับสวิตช์ดังนี้:

| การตั้งค่า | สถานะเริ่มต้น | เปลี่ยนเป็น | วิธีทำ | เหตุผล |
|-----------|--------------|------------|--------|--------|
| **Chat** | OFF | OFF ⚪ | ปล่อยไว้ | ไม่ต้องเปลี่ยน |
| **Greeting message** | ON 🟢 | OFF ⚪ | คลิกสวิตช์ให้เป็นสีเทา | ปิดข้อความทักทาย |
| **Webhooks** | OFF | **ON** 🟢 | **คลิกสวิตช์ให้เป็นสีเขียว** | **สำคัญที่สุด!** เปิดเพื่อให้ LINE ส่งข้อมูลมาที่ n8n |
| **Auto-response messages** | ON 🟢 | OFF ⚪ | คลิกสวิตช์ให้เป็นสีเทา | ปิดการตอบกลับอัตโนมัติ |

#### ผลลัพธ์ที่ถูกต้อง:

หลังจากปรับเสร็จ ควรเป็นดังนี้:
- ⚪ Chat: OFF
- ⚪ Greeting message: OFF
- 🟢 **Webhooks: ON** ← ต้องเป็นสีเขียว!
- ⚪ Auto-response messages: OFF

**หมายเหตุ:** การตั้งค่าจะบันทึกอัตโนมัติ ไม่ต้องกดปุ่ม Save

**เหตุผลที่ต้องตั้งค่าแบบนี้:**
- ถ้าเปิด **Greeting message** หรือ **Auto-response** ไว้ → Bot จะตอบข้อความเองตาม LINE default ทำให้ n8n ไม่สามารถควบคุมได้
- ต้องเปิด **Webhooks** → เพื่อให้ LINE Platform ส่งข้อมูลข้อความ (events) มาที่ Webhook URL ที่เราตั้งไว้ (ngrok → n8n)

### 1.8 จด Bot Basic ID / QR Code

กลับไปที่ LINE Developers Console > Messaging API tab:

- **Bot basic ID**: `@xxx-xxxxx` (ใช้ค้นหา Bot)
- **QR Code**: สแกนด้วยมือถือเพื่อเพิ่ม Bot เป็นเพื่อน

**ทำตอนนี้เลย:**
1. สแกน QR Code ด้วย LINE App ในมือถือ
2. เพิ่ม Bot เป็นเพื่อน (จะใช้ทดสอบในภายหลัง)

---

## 2. ติดตั้งและตั้งค่า ngrok

### 2.1 สมัครบัญชี ngrok

1. ไปที่ [https://ngrok.com/](https://ngrok.com/)
2. คลิก **Sign up for free**
3. สมัครด้วย Email หรือ GitHub/Google
4. ยืนยัน Email

### 2.2 ติดตั้ง ngrok

#### Windows (Chocolatey)
```powershell
choco install ngrok
```

#### Windows (Manual)
1. ดาวน์โหลดจาก [https://ngrok.com/download](https://ngrok.com/download)
2. แตกไฟล์ zip
3. ย้าย `ngrok.exe` ไปที่ต้องการ (เช่น `C:\ngrok\`)
4. เพิ่ม path ใน Environment Variables

#### macOS (Homebrew)
```bash
brew install ngrok
```

#### Linux (apt)
```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | \
  sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && \
  echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | \
  sudo tee /etc/apt/sources.list.d/ngrok.list && \
  sudo apt update && sudo apt install ngrok
```

### 2.3 ตั้งค่า Authtoken

1. Login เข้า [ngrok Dashboard](https://dashboard.ngrok.com/)
2. ไปที่ **Your Authtoken**
3. คัดลอก Authtoken
4. รันคำสั่ง:

```bash
ngrok config add-authtoken YOUR_AUTHTOKEN_HERE
```

### 2.4 ทดสอบ ngrok

```bash
ngrok http 5678
```

ถ้าสำเร็จจะเห็น:

```
Session Status                online
Account                       your-email@example.com
Version                       3.x.x
Region                        Asia Pacific (ap)
Forwarding                    https://xxxx-xx-xx-xx-xx.ngrok-free.app -> http://localhost:5678

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

**จด URL นี้ไว้**: `https://xxxx-xx-xx-xx-xx.ngrok-free.app`

---

## 3. รัน n8n

### 3.1 รัน n8n ด้วย Docker

#### Terminal 1 - รัน n8n

```bash
docker run -it --rm --name n8n \
  -p 5678:5678 \
  -e N8N_SECURE_COOKIE=false \
  n8nio/n8n
```

เปิด browser ไปที่: [http://localhost:5678](http://localhost:5678)

### 3.2 รัน ngrok

#### Terminal 2 - รัน ngrok

```bash
ngrok http 5678
```

### 3.3 จด URLs

| Service | URL |
|---------|-----|
| n8n Local | `http://localhost:5678` |
| n8n Public (ngrok) | `https://xxxx.ngrok-free.app` |
| Webhook URL | `https://xxxx.ngrok-free.app/webhook/gold-bot` |

---

## 4. ตั้งค่า LINE Webhook

### 4.1 สร้าง Workflow ใน n8n ก่อน

> ต้องสร้าง Webhook Node ก่อน เพื่อให้ n8n รู้จัก path

1. เปิด n8n
2. สร้าง Workflow ใหม่
3. เพิ่ม **Webhook** Node
4. ตั้งค่า:
   - HTTP Method: `POST`
   - Path: `gold-bot`
5. คลิก **Listen for Test Event** หรือ **Test workflow**

### 4.2 ตั้งค่าใน LINE Developers Console

1. ไปที่ [LINE Developers Console](https://developers.line.biz/console/)
2. เลือก Channel ของคุณ
3. ไปที่ **Messaging API** tab
4. เลื่อนไปที่ **Webhook settings**

#### ใส่ Webhook URL

```
https://xxxx.ngrok-free.app/webhook/gold-bot
```

⚠️ **แทนที่ `xxxx.ngrok-free.app` ด้วย URL จาก ngrok ของคุณ**

#### เปิดใช้งาน Webhook

1. เปิด **Use webhook** ให้เป็น ON
2. คลิก **Verify**

#### ผลลัพธ์ที่ถูกต้อง

```
✅ Success
```

ถ้าเห็น Error:
- ตรวจสอบว่า n8n ทำงานอยู่
- ตรวจสอบว่า ngrok ทำงานอยู่
- ตรวจสอบ URL ให้ถูกต้อง

---

## 5. ทดสอบระบบ

### 5.1 เพิ่ม Bot เป็นเพื่อน

1. ใน LINE Developers Console > Messaging API tab
2. สแกน **QR Code** ด้วย LINE App
3. เพิ่มเป็นเพื่อน

### 5.2 ทดสอบส่งข้อความ

1. เปิด LINE App
2. เปิดแชทกับ Bot
3. พิมพ์: `ราคาทอง`
4. Bot ควรตอบกลับด้วยราคาทองล่าสุด

### 5.3 ตรวจสอบ Execution ใน n8n

1. ไปที่ n8n
2. คลิก **Executions** (ซ้ายมือ)
3. ดูผลการทำงานของ Workflow

---

## 🔧 Troubleshooting

### ปัญหา: Webhook Verify ไม่ผ่าน

**สาเหตุ**: n8n ไม่ได้รับ request

**แก้ไข**:
1. ตรวจสอบว่า n8n ทำงานอยู่
2. ตรวจสอบว่า ngrok ทำงานอยู่
3. ตรวจสอบ URL ให้ถูกต้อง (รวม `/webhook/gold-bot`)
4. ใน n8n ต้องกด **Test workflow** หรือ **Listen for Test Event**

### ปัญหา: Bot ไม่ตอบกลับ

**สาเหตุที่เป็นไปได้**:
1. Workflow ไม่ถูกต้อง
2. Channel Access Token ผิด
3. Auto-reply ยังเปิดอยู่

**แก้ไข**:
1. ดู Execution log ใน n8n
2. ตรวจสอบ Token
3. ปิด Auto-reply ใน LINE Official Account Manager

### ปัญหา: Error 401 Unauthorized

**สาเหตุ**: Channel Access Token ไม่ถูกต้อง

**แก้ไข**:
1. ไป LINE Developers Console
2. Issue Token ใหม่
3. อัพเดท Token ใน n8n

### ปัญหา: ngrok URL เปลี่ยน

**สาเหตุ**: รัน ngrok ใหม่ (Free plan URL จะเปลี่ยน)

**แก้ไข**:
1. จด URL ใหม่จาก ngrok
2. อัพเดท Webhook URL ใน LINE Console
3. Verify อีกครั้ง

---

## 📝 Checklist ก่อนส่งงาน

- [ ] สร้าง LINE Channel แล้ว
- [ ] ได้ Channel Access Token แล้ว
- [ ] ติดตั้ง ngrok แล้ว
- [ ] รัน n8n + ngrok ได้
- [ ] สร้าง Workflow ครบทุก Node
- [ ] ตั้งค่า Webhook URL ใน LINE แล้ว
- [ ] ทดสอบส่งข้อความ "ราคาทอง" แล้ว Bot ตอบ
- [ ] Export workflow.json แล้ว
- [ ] Push ขึ้น GitHub แล้ว

---

## 📞 ต้องการความช่วยเหลือ?

- สร้าง Issue ใน Repository
- ถามใน Discord/LINE Group ของวิชา
- ติดต่ออาจารย์ผู้สอน
