অবশ্যই! নিচে ধাপে ধাপে শুরু থেকে GitLab Runner সেটআপ, রেজিস্ট্রেশন, সমস্যা ও সমাধান সবকিছু সাজানো ডকুমেন্ট দিলাম—যা আপনার জন্য ফিউচারে রেফারেন্স হিসেবে খুব কাজে লাগবে।

---

# GitLab Runner Setup and Troubleshooting Guide (Step by Step)

---

## ১. GitLab Runner কি?

GitLab Runner হলো একটি এজেন্ট যা GitLab CI/CD pipeline এর কাজগুলো চালায়। এটি আপনার সার্ভারে ইনস্টল করা হয় এবং GitLab এর সাথে কানেক্ট করে pipeline jobs রান করে।

---

## ২. GitLab Runner ইনস্টলেশন

### Ubuntu এ ইনস্টলেশন:

```bash
# GitLab Runner official repo যুক্ত করুন
curl -LO https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh
sudo bash script.deb.sh

# GitLab Runner ইনস্টল করুন
sudo apt-get install gitlab-runner

# we can install specific gitlab-runner
apt-cache madison gitlab-runner
sudo apt install gitlab-runner=17.7.1-1 gitlab-runner-helper-images=17.7.1-1
```

---

## ৩. Runner রেজিস্ট্রেশন

### ৩.১ রেজিস্ট্রেশন করার কমান্ড:

```bash
sudo gitlab-runner register
```

### ৩.২ রেজিস্ট্রেশন সময় যা দিতে হবে:

* **GitLab URL:**
  উদাহরণ: `https://gitlab.com`

* **Registration token:**
  GitLab প্রজেক্ট এর Settings > CI/CD > Runners > Registration token থেকে নিবেন।

* **Runner এর নাম:**
  যেকোনো স্মার্ট নাম যেমন `sony-runner`, `ubuntu2204-runner`

* **Executor টাইপ:**
  `shell` (সাধারণত local shell এর জন্য)

---

## ৪. রেজিস্ট্রেশন সফল হলে

* কনফিগারেশন `/etc/gitlab-runner/config.toml` ফাইলে সেভ হয়।
* `[[runners]]` সেকশন সেখানে যুক্ত হয়।
* Runner এখন GitLab থেকে জব নিতে পারে।

---

## ৫. GitLab Runner সার্ভিস চালু ও চেক করা

```bash
sudo systemctl start gitlab-runner
sudo systemctl enable gitlab-runner
sudo systemctl status gitlab-runner
```

---

## ৬. সাধারণ সমস্যা এবং সমাধান

### সমস্যা: Job fail হচ্ছে এবং Permission denied এরর দেখাচ্ছে

**এরর:**

```
mkdir: cannot create directory ‘/home/builds’: Permission denied
ERROR: Job failed: exit status 1
```

### কারণ:

* Runner `/home/builds` ডিরেক্টরিতে কোড ক্লোন ও রান করতে চাচ্ছে কিন্তু permission নেই।

### সমাধান:

#### ৬.১ Build Directory পরিবর্তন করুন

`config.toml` এ `builds_dir` সেট করুন যেখানে permission আছে।

```bash
sudo nano /etc/gitlab-runner/config.toml
```

`[[runners]]` সেকশনে যোগ করুন:

```toml
builds_dir = "/home/gitlab-runner/builds"
```

#### ৬.২ ডিরেক্টরি তৈরি ও permission দিন

```bash
sudo mkdir -p /home/gitlab-runner/builds
sudo chown -R gitlab-runner:gitlab-runner /home/gitlab-runner/builds
```

#### ৬.৩ Runner রিস্টার্ট দিন

```bash
sudo systemctl restart gitlab-runner
```

---

### সমস্যা: `config.toml` এ `[[runners]]` সেকশন নেই

### কারণ:

* Runner হয়তো রেজিস্টার হয়নি।

### সমাধান:

* Runner রেজিস্টার করুন আবার:

```bash
sudo gitlab-runner register
```

---

## ৭. GitLab এ Runner চেক করা

* GitLab প্রজেক্টে গিয়ে Settings > CI/CD > Runners দেখুন।
* আপনার Runner `active` এবং `online` আছে কিনা নিশ্চিত করুন।

---

## ৮. Pipeline `.gitlab-ci.yml` ফাইল লিখুন

উদাহরণ:

```yaml
test_script:
  script:
    - make test
```

---

## ৯. রান হওয়ার পেছনের প্রসেস

* Runner কোড ক্লোন করে নির্দিষ্ট `builds_dir` এ।
* `before_script` (যদি থাকে) রান হয় (যেখানে dependency ইনস্টল করা যায়)।
* তারপর `script` কমান্ডগুলো রান হয়।
* সফল হলে CI pipeline এগিয়ে যায়।

---

## ১০. ভবিষ্যতের জন্য টিপস

* সব সময় Runner এর ইউজার (যেমন `gitlab-runner`) কে ডিরেক্টরির permission দিন।
* `builds_dir` নিজের রানারের হোম ডিরেক্টরির ভিতরে রাখুন।
* Runner রিস্টার্ট দেওয়া ভুলবেন না যেকোনো কনফিগারেশন পরিবর্তনের পরে।

---

# উপসংহার

এই ডকুমেন্টেশন আপনার GitLab Runner setup, common সমস্যাগুলো ও তাদের সমাধান সহজে বুঝতে ও প্রয়োগ করতে সাহায্য করবে।

আপনার যদি আরও প্রশ্ন থাকে, আমি আছি সাহায্যের জন্য! 😊

---

আপনি চাইলে আমি এই ডকুমেন্ট টেক্সট ফাইল বা Markdown ফাইল আকারে তৈরি করে দিতে পারি। বলবেন!
