---
title: "Worklog Tuần 7"
date: "2025-11-30"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Xây dựng frontend application với HTML, CSS, và JavaScript.
* Tích hợp frontend với backend APIs (Amplify).
* Triển khai responsive UI design và user interactions.
* Kết nối Cognito cho user authentication.
* Deploy frontend sang S3 và CloudFront.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập Week 6 infrastructure và APIs <br> - Lập kế hoạch frontend UI/UX design <br> - Thiết lập project structure và dependencies                                                                                                                                                 | 29/10/2025   | 29/10/2025      | Week 6 Architecture                       |
| 2   | - **Frontend UI Development Part 1:** <br>&emsp; + Tạo HTML structure (header, nav, main, footer) <br>&emsp; + Thiết kế responsive layout với CSS Grid/Flexbox <br>&emsp; + Triển khai Bootstrap/Tailwind CSS framework <br>&emsp; + Tạo navigation menu và header component        | 30/10/2025   | 30/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Frontend UI Development Part 2:** <br>&emsp; + Xây dựng dashboard/main content area <br>&emsp; + Tạo form components cho user input <br>&emsp; + Thiết kế data display tables/cards <br>&emsp; + Triển khai modal dialogs và notifications                                      | 01/11/2025   | 01/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Frontend Authentication:** <br>&emsp; + Tích hợp Cognito cho user login/signup <br>&emsp; + Tạo login form và authentication flow <br>&emsp; + Lưu JWT tokens một cách secure <br>&emsp; + Triển khai logout functionality <br>&emsp; + Thêm session management                 | 02/11/2025   | 02/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **API Integration:** <br>&emsp; + Kết nối frontend tới Amplify APIs <br>&emsp; + Triển khai fetch/axios cho HTTP requests <br>&emsp; + Xử lý API responses và errors <br>&emsp; + Hiển thị dynamic data từ backend <br>&emsp; + Triển khai loading states và spinners             | 03/11/2025   | 03/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Frontend Optimization & Testing:** <br>&emsp; + Tối ưu hóa images và assets <br>&emsp; + Minify CSS và JavaScript <br>&emsp; + Test responsive design trên mobile devices <br>&emsp; + Test browser compatibility <br>&emsp; + Thực hiện accessibility audit (WCAG)             | 04/11/2025   | 04/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Deployment & Integration:** <br>&emsp; + Deploy frontend tới S3 bucket <br>&emsp; + Cấu hình CloudFront distribution <br>&emsp; + Thiết lập custom domain với Route 53 <br>&emsp; + Test complete application flow end-to-end <br>&emsp; + Ghi chép frontend deployment process | 05/11/2025   | 05/11/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

## Frontend Technology Stack

**HTML Framework:**
- Semantic HTML5 structure
- Accessibility best practices
- Meta tags cho SEO

**CSS Frameworks:**
- Bootstrap hoặc Tailwind CSS
- Custom CSS cho unique designs
- SCSS/SASS cho advanced styling (optional)

**JavaScript Libraries:**
- Amplify JS cho AWS integration
- Amazon Cognito SDK
- Fetch hoặc Axios cho HTTP requests
- Optional: React/Vue cho advanced apps

**Build Tools:**
- Webpack hoặc Vite cho bundling
- npm/yarn cho package management
- ESLint cho code quality
- Prettier cho code formatting

---

## Deployment Process

**Build & Optimize:**
- Build production bundle
- Minify CSS và JavaScript
- Optimize images
- Generate source maps (optional)

**CloudFront Setup:**
- Tạo distribution pointing tới S3
- Set origin domain tới S3 website endpoint
- Configure cache behavior
- Add custom domain/SSL certificate
- Set default root object tới index.html
- Configure 404 error handling

---

## Common Frontend Components

**Navigation Bar:**
- Logo và branding
- Links tới main sections
- User profile menu
- Search box (optional)
- Mobile toggle menu

**Cards:**
- Image placeholder
- Title và description
- Action buttons (Edit, Delete)
- Metadata (date, author)

**Forms:**
- Input validation
- Error messages
- Success feedback
- Loading state during submission

**Tables:**
- Column headers
- Sortable columns
- Pagination
- Row selection (checkboxes)
- Action buttons per row

**Alerts/Notifications:**
- Success messages
- Error messages
- Warning messages
- Info messages
- Auto-dismiss hoặc manual close

---

## Ghi Chú:

* **Bắt đầu Đơn giản:** Bắt đầu với basic HTML/CSS, thêm interactivity với JavaScript
* **User Experience:** Tập trung vào intuitive navigation và clear feedback
* **Mobile First:** Thiết kế cho mobile first, sau đó enhance cho larger screens
* **Accessibility:** Làm cho app sử dụng được cho tất cả users, bao gồm những người có disabilities
* **Performance:** Tối ưu hóa assets và minimize API calls cho fast loading
* **Testing:** Test kỹ lưỡng trên different devices và browsers trước deployment