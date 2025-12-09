---
title: "Week 8 Worklog"
date: "2025-11-30"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Integrate backend APIs into frontend application.
* Connect Amplify APIs with database operations.
* Implement real-time data synchronization.
* Handle authentication and authorization in API calls.
* Test complete end-to-end application workflow.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                          | Start Date | End Date   | Resources                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 6-7 frontend and backend setup <br> - Plan API integration architecture <br> - Identify all API endpoints needed                                                                                                                                                                                 | 06/11/2025 | 06/11/2025 | Week 6-7 Documentation                    |
| 2   | - **API Configuration & Setup:** <br>&emsp; + Configure Amplify API client <br>&emsp; + Set up API request/response interceptors <br>&emsp; + Implement error handling middleware <br>&emsp; + Configure authentication headers <br>&emsp; + Set up API base URL and endpoints                                 | 07/11/2025 | 07/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **CRUD Operations Integration:** <br>&emsp; + Integrate GET endpoints (list, detail view) <br>&emsp; + Integrate POST endpoints (create records) <br>&emsp; + Integrate PUT endpoints (update records) <br>&emsp; + Integrate DELETE endpoints (remove records) <br>&emsp; + Handle loading and error states | 08/11/2025 | 08/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Data Display & Rendering:** <br>&emsp; + Display API data in tables <br>&emsp; + Render data in cards/components <br>&emsp; + Implement pagination for large datasets <br>&emsp; + Add sorting and filtering <br>&emsp; + Real-time data updates on form submit                                            | 09/11/2025 | 09/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Form Integration with APIs:** <br>&emsp; + Connect form submission to POST/PUT APIs <br>&emsp; + Validate data before API calls <br>&emsp; + Show loading indicators during requests <br>&emsp; + Display success/error messages <br>&emsp; + Clear form after successful submission                       | 10/11/2025 | 10/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Authentication & Authorization:** <br>&emsp; + Pass JWT tokens in API headers <br>&emsp; + Handle 401 unauthorized errors <br>&emsp; + Implement role-based access control <br>&emsp; + Show/hide UI based on user permissions <br>&emsp; + Refresh tokens on expiry                                       | 11/11/2025 | 11/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Testing & Deployment:** <br>&emsp; + Test all API endpoints from frontend <br>&emsp; + Verify CORS configuration <br>&emsp; + Load test with multiple concurrent requests <br>&emsp; + Test error scenarios <br>&emsp; + Deploy to S3/CloudFront and verify <br>&emsp; + Document API integration points   | 12/11/2025 | 12/11/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

### API Configuration

**Setup Amplify API:**
- Initialize Amplify with API endpoint
- Configure GraphQL/REST API
- Set up request interceptors
- Add authentication headers automatically
- Handle token refresh

**Error Handling:**
- Network errors → Show offline message
- 401 Unauthorized → Redirect to login
- 403 Forbidden → Show permission denied
- 404 Not Found → Show not found message
- 500 Server Error → Show error with retry option

### Request/Response Management

**Loading States:**
- Show spinner during API requests
- Disable form submit button during submission
- Display "Loading..." message in tables
- Show skeleton loaders for data placeholders

**Success/Error Messages:**
- Toast notification for successful operations
- Alert modal for critical errors
- Inline error messages for form validation
- Success message before closing form
---

## Notes:

* **Integration First:** Test APIs using tools (Postman) before frontend integration
* **Error Messages:** Provide clear, user-friendly error messages
* **Loading States:** Always show loading indicator during API calls
* **Token Management:** Implement proper token refresh and expiry handling
* **CORS Setup:** Ensure CORS is properly configured on backend
* **Documentation:** Document all API endpoints and their parameters
* **Testing:** Test all API integration points thoroughly before deployment