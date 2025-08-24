📘 GitLab Setup Guide (README.md)
# 🚀 GitLab Setup Guide

এই ডকুমেন্টে লোকাল PC থেকে GitLab এ কানেক্ট করার সম্পূর্ণ Flow দেওয়া আছে।  
প্রতিবার নতুন প্রজেক্ট সেটআপ করতে হলে এই স্টেপগুলো ফলো করলেই হবে ✅

---

## 1️⃣ Install Git
```bash
git --version


যদি Git না থাকে:

Linux: sudo apt install git -y

Windows: Download Git

2️⃣ Configure User
git config --global user.name "তোমার নাম"
git config --global user.email "তোমার GitLab ইমেইল"

3️⃣ Generate SSH Key
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"


ডিফল্ট path ব্যবহার করো: ~/.ssh/id_rsa

চাইলে passphrase খালি রাখতে পারো

4️⃣ Add SSH Key to Agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

5️⃣ Copy Public Key
cat ~/.ssh/id_rsa.pub


👉 Output কপি করো এবং GitLab এ যোগ করো:
User Settings → SSH Keys → Add key

6️⃣ Test Connection
ssh -T git@gitlab.com


✅ যদি সফল হয়:
Welcome to GitLab, @username!

7️⃣ Clone Repository
git clone git@gitlab.com:username/repo-name.git

8️⃣ Push Existing Project
git init
git remote add origin git@gitlab.com:username/repo-name.git
git add .
git commit -m "first commit"
git branch -M main
git push -u origin main

🔄 Typical Branch Flow

main → Stable/Production

release/ → Release preparation

feature/ → New feature dev

Example:

# Create a feature branch
git checkout -b feature/login-api

# Work on it, then push
git push -u origin feature/login-api

# Merge via GitLab → main

✅ Summary

Install Git & set config

Generate & add SSH key

Clone / Push repo

Follow branch flow


---

👉 এই ফাইলটা তুমি সরাসরি তোমার repo তে `README.md` নামে রাখলেই একটা সম্পূর্ণ গাইড হয়ে যাবে 🚀  

তুমি চাইলে আমি সাথে সাথে **repo structure (README.md + scripts/**) বানিয়ে দিতে পারি, নাকি শুধু এই README.md দিয়েই শুরু করবে?
