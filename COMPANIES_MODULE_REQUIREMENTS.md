# Companies Module Implementation Requirements

## Overview
This document outlines the requirements for implementing a Companies module with full CRUD (Create, Read, Update, Delete) operations accessible to administrators.

## Repository Context
**Important:** This repository (`alphaMain`) is a deployment/build repository containing compiled Angular files. The actual implementation must be done in the source code repository (likely `frontendPrinter` or similar).

## Translation Files
✅ **Completed** - Translation files have been added for the Companies module in 4 languages:
- Spanish (`es.json`)
- English (`en.json`)
- German (`de.json`)
- French (`fr.json`)

All necessary translation keys for the CRUD interface are now available.

## Backend Requirements

### API Endpoints
The backend should provide the following REST API endpoints:

1. **GET /api/companies** - List all companies (with pagination, filtering, and sorting)
2. **GET /api/companies/:id** - Get company details by ID
3. **POST /api/companies** - Create a new company
4. **PUT /api/companies/:id** - Update an existing company
5. **DELETE /api/companies/:id** - Delete a company

### Company Model
The Company entity should include at minimum:

```typescript
interface Company {
  id: string | number;
  name: string;           // Required
  description?: string;
  address?: string;
  phone?: string;
  email?: string;         // Valid email format
  website?: string;       // Valid URL format
  status: 'active' | 'inactive';
  createdAt: Date;
  updatedAt: Date;
}
```

### Authentication & Authorization
- Only users with **Administrator** role should have access to the Companies module
- Implement proper authentication middleware
- Validate user permissions before allowing CRUD operations

## Frontend Requirements (Source Code Repository)

### Module Structure
Create an Angular module with the following structure:

```
src/app/pages/companies/
├── companies.module.ts
├── companies-routing.module.ts
├── components/
│   ├── companies-list/
│   │   ├── companies-list.component.ts
│   │   ├── companies-list.component.html
│   │   ├── companies-list.component.scss
│   │   └── companies-list.component.spec.ts
│   ├── company-form/
│   │   ├── company-form.component.ts
│   │   ├── company-form.component.html
│   │   ├── company-form.component.scss
│   │   └── company-form.component.spec.ts
│   └── company-details/
│       ├── company-details.component.ts
│       ├── company-details.component.html
│       ├── company-details.component.scss
│       └── company-details.component.spec.ts
├── services/
│   └── companies.service.ts
└── models/
    └── company.model.ts
```

### Required Components

#### 1. Companies List Component
- Display a table/grid of all companies
- Implement search functionality
- Implement filtering by status (Active/Inactive)
- Implement sorting by columns (Name, Created Date, etc.)
- Implement pagination
- Show action buttons: View, Edit, Delete
- Include "Add Company" button

#### 2. Company Form Component
- Reusable form for both Create and Edit operations
- Form fields:
  - Company Name (required, text input)
  - Description (optional, textarea)
  - Address (optional, text input)
  - Phone (optional, with validation)
  - Email (optional, with email validation)
  - Website (optional, with URL validation)
  - Status (required, dropdown: Active/Inactive)
- Form validation using Angular Reactive Forms
- Display validation errors using translation keys
- Save and Cancel buttons

#### 3. Company Details Component
- Display full company information in read-only view
- Include Edit and Delete action buttons
- Show created and updated timestamps

### Service Layer

#### Companies Service
Create a service to handle all API communications:

```typescript
@Injectable({
  providedIn: 'root'
})
export class CompaniesService {
  getCompanies(params?: QueryParams): Observable<CompanyListResponse>;
  getCompany(id: string): Observable<Company>;
  createCompany(company: Company): Observable<Company>;
  updateCompany(id: string, company: Company): Observable<Company>;
  deleteCompany(id: string): Observable<void>;
}
```

### Routing Configuration
Add routes in `companies-routing.module.ts`:

```typescript
const routes: Routes = [
  {
    path: '',
    component: CompaniesListComponent
  },
  {
    path: 'new',
    component: CompanyFormComponent,
    canActivate: [AdminGuard]
  },
  {
    path: ':id',
    component: CompanyDetailsComponent,
    canActivate: [AdminGuard]
  },
  {
    path: ':id/edit',
    component: CompanyFormComponent,
    canActivate: [AdminGuard]
  }
];
```

### Navigation Menu
Add "Companies" menu item to the admin navigation:
- Use translation key: `"Companies"`
- Icon suggestion: `business` or `domain`
- Route: `/companies`
- Visible only to administrators

### User Experience Features

#### Success/Error Messages
- Show toast/snackbar notifications for:
  - Successful create, update, delete operations
  - Error messages for failed operations
- Use translation keys from the i18n files

#### Confirmation Dialogs
- Implement confirmation dialog before deleting a company
- Use translation key: `"Are you sure you want to delete this company?"`

#### Loading States
- Show loading indicators during API calls
- Disable form buttons while submitting

#### Error Handling
- Handle network errors gracefully
- Display user-friendly error messages
- Implement retry mechanisms where appropriate

## Testing Requirements

### Unit Tests
- Test all components with proper mocking
- Test the service layer with HTTP client mocks
- Test form validations
- Test route guards

### Integration Tests
- Test the complete CRUD flow
- Test navigation between components
- Test authorization/authentication

### E2E Tests
- Test the complete user journey
- Test CRUD operations end-to-end
- Test error scenarios

## Security Considerations

1. **Authentication**: Verify user is logged in before accessing any company routes
2. **Authorization**: Ensure only administrators can access the Companies module
3. **Input Validation**: Validate all inputs on both frontend and backend
4. **XSS Prevention**: Sanitize user inputs to prevent cross-site scripting
5. **CSRF Protection**: Implement CSRF tokens for state-changing operations
6. **SQL Injection**: Use parameterized queries in the backend

## Accessibility Requirements

- Ensure ARIA labels are properly set
- Maintain keyboard navigation support
- Ensure proper contrast ratios
- Support screen readers
- Use semantic HTML elements

## Performance Considerations

- Implement lazy loading for the Companies module
- Use virtual scrolling for large lists
- Implement debouncing for search functionality
- Cache frequently accessed data
- Optimize bundle size

## Deployment Checklist

- [ ] Implement backend API endpoints
- [ ] Create Angular module and components
- [ ] Implement service layer
- [ ] Add routing configuration
- [ ] Update navigation menu
- [ ] Add form validations
- [ ] Implement error handling
- [ ] Add success/error notifications
- [ ] Implement authorization guards
- [ ] Write unit tests
- [ ] Write integration tests
- [ ] Write E2E tests
- [ ] Update API documentation
- [ ] Perform security audit
- [ ] Test accessibility compliance
- [ ] Build and deploy to staging
- [ ] Perform UAT (User Acceptance Testing)
- [ ] Deploy to production
- [ ] Update user documentation

## Notes

- All text strings should use the translation keys from the i18n files
- Follow the existing code style and patterns in the application
- Ensure responsive design for mobile devices
- Consider adding export functionality (CSV, PDF) in future iterations
- Consider adding bulk operations in future iterations
