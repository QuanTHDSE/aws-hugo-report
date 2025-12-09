---
title: "Week 7 Worklog"
date: "2025-11-30"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Build frontend application with HTML, CSS, and JavaScript.
* Integrate frontend with backend APIs (Amplify).
* Implement responsive UI design and user interactions.
* Connect to Cognito for user authentication.
* Deploy frontend to S3 and CloudFront.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                             | Start Date | End Date   | Resources                                 |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 6 infrastructure and APIs <br> - Plan frontend UI/UX design <br> - Set up project structure and dependencies                                                                                                                                                        | 29/10/2025 | 29/10/2025 | Week 6 Architecture                       |
| 2   | - **Frontend UI Development Part 1:** <br>&emsp; + Create HTML structure (header, nav, main, footer) <br>&emsp; + Design responsive layout with CSS Grid/Flexbox <br>&emsp; + Implement Bootstrap/Tailwind CSS framework <br>&emsp; + Create navigation menu and header component | 30/10/2025 | 30/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Frontend UI Development Part 2:** <br>&emsp; + Build dashboard/main content area <br>&emsp; + Create form components for user input <br>&emsp; + Design data display tables/cards <br>&emsp; + Implement modal dialogs and notifications                                      | 01/11/2025 | 01/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Frontend Authentication:** <br>&emsp; + Integrate Cognito for user login/signup <br>&emsp; + Create login form and authentication flow <br>&emsp; + Store JWT tokens securely <br>&emsp; + Implement logout functionality <br>&emsp; + Add session management                 | 02/11/2025 | 02/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **API Integration:** <br>&emsp; + Connect frontend to Amplify APIs <br>&emsp; + Implement fetch/axios for HTTP requests <br>&emsp; + Handle API responses and errors <br>&emsp; + Display dynamic data from backend <br>&emsp; + Implement loading states and spinners          | 03/11/2025 | 03/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Frontend Optimization & Testing:** <br>&emsp; + Optimize images and assets <br>&emsp; + Minify CSS and JavaScript <br>&emsp; + Test responsive design on mobile devices <br>&emsp; + Test browser compatibility <br>&emsp; + Perform accessibility audit (WCAG)               | 04/11/2025 | 04/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Deployment & Integration:** <br>&emsp; + Deploy frontend to S3 bucket <br>&emsp; + Configure CloudFront distribution <br>&emsp; + Set up custom domain with Route 53 <br>&emsp; + Test complete application flow end-to-end <br>&emsp; + Document frontend deployment process | 05/11/2025 | 05/11/2025 | <https://cloudjourney.awsstudygroup.com/> |


---

## Frontend Technology Stack

**HTML Framework:**
- Semantic HTML5 structure
- Accessibility best practices
- Meta tags for SEO

**CSS Frameworks:**
- Bootstrap or Tailwind CSS
- Custom CSS for unique designs
- SCSS/SASS for advanced styling (optional)

**JavaScript Libraries:**
- Amplify JS for AWS integration
- Amazon Cognito SDK
- Fetch or Axios for HTTP requests
- Optional: React/Vue for advanced apps

**Build Tools:**
- Webpack or Vite for bundling
- npm/yarn for package management
- ESLint for code quality
- Prettier for code formatting

---

## Deployment Process

**Build & Optimize:**
- Build production bundle
- Minify CSS and JavaScript
- Optimize images
- Generate source maps (optional)
- 

**CloudFront Setup:**
- Create distribution pointing to S3
- Set origin domain to S3 website endpoint
- Configure cache behavior
- Add custom domain/SSL certificate
- Set default root object to index.html
- Configure 404 error handling

---

## Common Frontend Components

**Navigation Bar:**
- Logo and branding
- Links to main sections
- User profile menu
- Search box (optional)
- Mobile toggle menu

**Cards:**
- Image placeholder
- Title and description
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
- Auto-dismiss or manual close

---

## Notes:

* **Start Simple:** Begin with basic HTML/CSS, add interactivity with JavaScript
* **User Experience:** Focus on intuitive navigation and clear feedback
* **Mobile First:** Design for mobile first, then enhance for larger screens
* **Accessibility:** Make your app usable for all users, including those with disabilities
* **Performance:** Optimize assets and minimize API calls for fast loading
* **Testing:** Test thoroughly on different devices and browsers before deployment