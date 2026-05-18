# 💻 n8n — Code Node (JavaScript) Guide

> n8n এ Code Node ব্যবহার করে নিজের JavaScript Logic দিয়ে Data Process করার গাইড।

---

## 📊 Workflow Overview

```
Manual Trigger
      │
      ▼
Edit Fields (Set)
[raw_price = "৳ 1,200.00"]
      │
      ▼
Code Node (JavaScript)
[clean_price = 1200]
      │
      ▼
Output দেখো
```

---

## ⚙️ Workflow সেটআপ

### ✅ ধাপ ১ — Manual Trigger ও Edit Fields যোগ করো

- **"Add first step"** এ ক্লিক করো
- **"Trigger Manually"** সিলেক্ট করো
- **Manual Trigger** এর **`+`** বাটনে ক্লিক করো
- **"set"** সার্চ করো → **"Edit Fields (Set)"** সিলেক্ট করো
- **"Add Field"** এ ক্লিক করো
- প্রয়োজনীয় **Value** সেট করো

> 💡 **Edit Fields কেন?** Code Node এ পাঠানোর আগে Test Data তৈরি করতে Edit Fields ব্যবহার করা হয়।

![Edit Fields Setup](https://imgur.com/1drD8Vl.png)

---

### ✅ ধাপ ২ — Code Node যোগ করো

- **Edit Fields** এর **`+`** বাটনে ক্লিক করো
- **"code"** সার্চ করো
- **"Code in JavaScript"** সিলেক্ট করো
- নিজের Logic অনুযায়ী Code লিখো

![Code Node JavaScript](https://imgur.com/zG51UZ2.png)

---

## 🧑‍💻 Code Node এ কোড লেখা

### উদাহরণ: Price থেকে Symbol ও Comma সরিয়ে সংখ্যা বের করো

```javascript
// Edit Fields থেকে আসা প্রতিটি Item এর জন্য Loop চলবে
for (const item of $input.all()) {

  // raw_price field থেকে value নাও
  const rawPrice = item.json.raw_price;

  // ৳, comma, space সরিয়ে clean করো
  const cleanPrice = parseFloat(
    rawPrice.replace(/[৳,\s]/g, "")
  );

  // নতুন field হিসেবে যোগ করো
  item.json.clean_price = cleanPrice;
}

// সব Item return করো
return $input.all();
```

### আরও কিছু কাজের উদাহরণ:

**নাম থেকে First Name বের করো:**
```javascript
for (const item of $input.all()) {
  const fullName = item.json.full_name;
  item.json.first_name = fullName.split(" ")[0];
}
return $input.all();
```

**তারিখ Format করো:**
```javascript
for (const item of $input.all()) {
  const date = new Date(item.json.created_at);
  item.json.formatted_date = date.toLocaleDateString("bn-BD");
}
return $input.all();
```

**সংখ্যা দিয়ে শর্ত যাচাই করো:**
```javascript
for (const item of $input.all()) {
  item.json.is_expensive = item.json.clean_price > 1000 ? "হ্যাঁ" : "না";
}
return $input.all();
```

---

### ✅ ধাপ ৩ — Execute করো এবং Output দেখো

- **Execute Workflow** বাটনে ক্লিক করো
- **Code in JavaScript** Node ওপেন করো
- Output এ **`clean_price`** Field দেখা যাবে

> ✔️ যদি সঠিকভাবে কাজ করে তাহলে:
> - **Input:** `"৳ 1,200.00"`
> - **Output:** `1200`

---

## 📚 Code Node সম্পর্কে জানো

### Code Node কী?

> n8n এ যখন বিল্ট-ইন Node দিয়ে কাজ হয় না, তখন **নিজের JavaScript Code** লিখে যেকোনো Logic চালানো যায়।

### Code Node এ কী কী করা যায়?

| কাজ | উদাহরণ |
|-----|---------|
| **Data পরিষ্কার করা** | Price থেকে Symbol সরানো |
| **Format বদলানো** | তারিখ বা সংখ্যার Format ঠিক করা |
| **নতুন Field তৈরি** | দুটো Field জুড়ে নতুন Field বানানো |
| **শর্ত যাচাই** | কোনো মান বড় বা ছোট কিনা চেক করা |
| **গণনা করা** | মোট দাম, ছাড়, VAT হিসাব করা |

### গুরুত্বপূর্ণ n8n Code Syntax:

```javascript
// সব Item পড়ো
$input.all()

// প্রথম Item পড়ো
$input.first()

// Item এর Data
item.json.field_name

// নতুন Field যোগ করো
item.json.new_field = "value";

// সব Item Return করো
return $input.all();
```

---

## 📋 Quick Reference

| ধাপ | Node | কাজ |
|-----|------|-----|
| ১ | Manual Trigger | Workflow শুরু করো |
| ১ | Edit Fields (Set) | Test Data তৈরি করো |
| ২ | Code (JavaScript) | নিজের Logic দিয়ে Data Process করো |
| ৩ | Execute | Output যাচাই করো |

### Code Node এর সুবিধা:

```
বিল্ট-ইন Node নেই? → Code Node দিয়ে যেকোনো কাজ করো
Complex Logic? → JavaScript এর পুরো শক্তি ব্যবহার করো
Data পরিষ্কার? → Regex, Parse, Format সব সম্ভব
```

---

*Code Node শিখে নিলে n8n এ এমন কোনো কাজ নেই যা করা সম্ভব না।*
