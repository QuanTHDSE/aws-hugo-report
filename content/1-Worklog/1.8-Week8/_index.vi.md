---
title: "Worklog Tuần 8"
date: "2025-11-30"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tích hợp backend APIs vào frontend application.
* Kết nối Amplify APIs với database operations.
* Triển khai real-time data synchronization.
* Xử lý authentication và authorization trong API calls.
* Test complete end-to-end application workflow.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                      | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập Week 6-7 frontend và backend setup <br> - Lập kế hoạch API integration architecture <br> - Xác định tất cả API endpoints cần thiết                                                                                                                                                                    | 06/11/2025   | 06/11/2025      | Tài liệu Week 6-7                         |
| 2   | - **API Configuration & Setup:** <br>&emsp; + Cấu hình Amplify API client <br>&emsp; + Thiết lập API request/response interceptors <br>&emsp; + Triển khai error handling middleware <br>&emsp; + Cấu hình authentication headers <br>&emsp; + Thiết lập API base URL và endpoints                             | 07/11/2025   | 07/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **CRUD Operations Integration:** <br>&emsp; + Tích hợp GET endpoints (list, detail view) <br>&emsp; + Tích hợp POST endpoints (create records) <br>&emsp; + Tích hợp PUT endpoints (update records) <br>&emsp; + Tích hợp DELETE endpoints (remove records) <br>&emsp; + Xử lý loading và error states       | 08/11/2025   | 08/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Data Display & Rendering:** <br>&emsp; + Hiển thị API data trong tables <br>&emsp; + Render data trong cards/components <br>&emsp; + Triển khai pagination cho large datasets <br>&emsp; + Thêm sorting và filtering <br>&emsp; + Real-time data updates on form submit                                    | 09/11/2025   | 09/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Form Integration with APIs:** <br>&emsp; + Kết nối form submission tới POST/PUT APIs <br>&emsp; + Validate data trước API calls <br>&emsp; + Hiển thị loading indicators during requests <br>&emsp; + Hiển thị success/error messages <br>&emsp; + Clear form sau successful submission                    | 10/11/2025   | 10/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Authentication & Authorization:** <br>&emsp; + Pass JWT tokens trong API headers <br>&emsp; + Xử lý 401 unauthorized errors <br>&emsp; + Triển khai role-based access control <br>&emsp; + Show/hide UI dựa trên user permissions <br>&emsp; + Refresh tokens on expiry                                    | 11/11/2025   | 11/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Testing & Deployment:** <br>&emsp; + Test tất cả API endpoints từ frontend <br>&emsp; + Xác minh CORS configuration <br>&emsp; + Load test với multiple concurrent requests <br>&emsp; + Test error scenarios <br>&emsp; + Deploy tới S3/CloudFront và verify <br>&emsp; + Ghi chép API integration points | 12/11/2025   | 12/11/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### API Configuration

**Setup Amplify API:**
- Initialize Amplify với API endpoint
- Configure GraphQL/REST API
- Thiết lập request interceptors
- Thêm authentication headers automatically
- Xử lý token refresh

**Error Handling:**
- Network errors → Hiển thị offline message
- 401 Unauthorized → Redirect to login
- 403 Forbidden → Hiển thị permission denied
- 404 Not Found → Hiển thị not found message
- 500 Server Error → Hiển thị error với retry option

### Request/Response Management

**Loading States:**
- Hiển thị spinner during API requests
- Disable form submit button during submission
- Display "Loading..." message trong tables
- Hiển thị skeleton loaders cho data placeholders

**Success/Error Messages:**
- Toast notification cho successful operations
- Alert modal cho critical errors
- Inline error messages cho form validation
- Success message trước closing form

---

## Ghi Chú:

* **Integration First:** Test APIs sử dụng tools (Postman) trước frontend integration
* **Error Messages:** Cung cấp clear, user-friendly error messages
* **Loading States:** Luôn show loading indicator during API calls
* **Token Management:** Implement proper token refresh và expiry handling
* **CORS Setup:** Đảm bảo CORS properly configured trên backend
* **Documentation:** Ghi chép tất cả API endpoints và parameters
* **Testing:** Test tất cả API integration points thoroughly trước deployment