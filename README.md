# 🛠️ Portfolio Admin

Welcome to the control center! 🕹️ This repository hosts the deployed admin portal for managing your portfolio content. It's the "behind-the-scenes" magic that keeps your CV up to date. ✨

## 🌟 What does it do?

- **Secure Login**: Only you can access your data via Supabase Auth. 🔐
- **Full CRUD**: Create, read, update, and delete everything from profile details to blog posts. 📝
- **Asset Management**: Upload profile pictures, project thumbnails, and certificates directly to Supabase Storage. 📂
- **Live Updates**: Changes show up on your portfolio site instantly! ⚡️

## 📦 Content Managed

You have full control over:
- **Profile**: Name, title, summary, and socials. 👤
- **Skills**: Categories, proficiency, and descriptions. 💪
- **Experience**: Your professional timeline and achievements. 🏢
- **Projects**: Showcase your best work with thumbnails and links. 🚀
- **Certificates**: Keep your credentials organized. 📜
- **Blog Posts**: Share your thoughts with the world! ✍️

## 🏗️ Technical Details

This repository is for the **deployed static site**. The source code lives in the main [web-cv](https://github.com/your-username/web-cv) monorepo under `apps/admin`. 📂

- **Backend**: Supabase (Auth, DB, Storage) ☁️
- **Frontend**: React + Vite + Tailwind CSS ⚛️🎨
- **Deployment**: GitHub Pages 🚀

## 🔐 Security First

- Uses **Row Level Security (RLS)** in Supabase to ensure only authorized admins can write data. 🛡️
- Never exposes service role keys in the browser. 🚫🔑
- Role-based access via the `app_admins` table. 🎟️

## 🚀 Development

If you want to work on the admin app locally, head over to the main monorepo and run:

```bash
npm run dev:admin
```

Happy managing! 🌈✨
