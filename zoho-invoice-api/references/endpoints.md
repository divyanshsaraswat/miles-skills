# Endpoint catalog — Zoho Invoice API v3

Base URL: `https://www.zohoapis.com/invoice/v3` (swap domain per data center).
Every path below is relative to that base. Verified-from-source paths are marked ✅; everything else follows Zoho's standard convention (lowercase, no hyphens, plural resource name) but **spot-check against the live doc page or OpenAPI file before relying on it in production** — see SKILL.md §4 for how.

Docs index for any section: `https://www.zoho.com/invoice/api/v3/{section-slug}/` (the slug is shown in the "doc section" column).

## Organizations — doc section: `organizations`
| Action | Method | Path |
|---|---|---|
| List organizations | GET | `/organizations` |
| Create an organization | POST | `/organizations` |
| Get an organization | GET | `/organizations/{organization_id}` |
| Update an organization | PUT | `/organizations/{organization_id}` |

## Items — doc section: `items`
| Action | Method | Path |
|---|---|---|
| Create an item | POST | `/items` |
| List items | GET | `/items` |
| Bulk fetch item details | GET | `/items/bulkdetails` |
| Retrieve an item | GET | `/items/{item_id}` |
| Update an item | PUT | `/items/{item_id}` |
| Delete an item | DELETE | `/items/{item_id}` |
| Update custom field in existing items | PUT | `/items/customfields` |
| Mark as active | POST | `/items/{item_id}/active` |
| Mark as inactive | POST | `/items/{item_id}/inactive` |

## Price Lists — doc section: `price-lists`
| Action | Method | Path |
|---|---|---|
| Create a Price List | POST | `/pricebooks` |
| List all Price Lists | GET | `/pricebooks` |
| Retrieve a Price List | GET | `/pricebooks/{pricebook_id}` |
| Update a Price List | PUT | `/pricebooks/{pricebook_id}` |
| Delete a Price List | DELETE | `/pricebooks/{pricebook_id}` |

## Contacts — doc section: `contacts` ✅ base path confirmed
| Action | Method | Path |
|---|---|---|
| Create a Contact | POST | `/contacts` |
| List Contacts | GET | `/contacts` |
| Get a Contact | GET | `/contacts/{contact_id}` |
| Update a Contact | PUT | `/contacts/{contact_id}` |
| Delete a Contact | DELETE | `/contacts/{contact_id}` |
| Mark as Active | POST | `/contacts/{contact_id}/active` |
| Mark as Inactive | POST | `/contacts/{contact_id}/inactive` |
| Enable Portal Access | POST | `/contacts/{contact_id}/portal/enable` |
| View All Client Reviews | GET | `/contacts/{contact_id}/reviews` |
| Enable Payment Reminders | POST | `/contacts/{contact_id}/paymentreminder/enable` |
| Disable Payment Reminders | POST | `/contacts/{contact_id}/paymentreminder/disable` |
| Email Statement | POST | `/contacts/{contact_id}/statements/email` |
| Email Contact | POST | `/contacts/{contact_id}/email` |
| List Comments | GET | `/contacts/{contact_id}/comments` |
| Add Additional Address | POST | `/contacts/{contact_id}/address` |
| Get Contact Addresses | GET | `/contacts/{contact_id}/address` |
| Edit Additional Address | PUT | `/contacts/{contact_id}/address/{address_id}` |
| Delete Additional Address | DELETE | `/contacts/{contact_id}/address/{address_id}` |
| List Refunds | GET | `/contacts/{contact_id}/refunds` |

## Contact Persons — doc section: `contact-persons`
| Action | Method | Path |
|---|---|---|
| Create a contact person | POST | `/contactpersons` |
| List contact persons | GET | `/contactpersons` |
| Get a contact person | GET | `/contactpersons/{contact_person_id}` |
| Update a contact person | PUT | `/contactpersons/{contact_person_id}` |
| Delete a contact person | DELETE | `/contactpersons/{contact_person_id}` |
| Mark as primary contact person | POST | `/contactpersons/{contact_person_id}/primary` |

## Estimates — doc section: `estimates`
| Action | Method | Path |
|---|---|---|
| Create an Estimate | POST | `/estimates` |
| List estimates | GET | `/estimates` |
| Get an estimate | GET | `/estimates/{estimate_id}` |
| Update an Estimate | PUT | `/estimates/{estimate_id}` |
| Delete an Estimate | DELETE | `/estimates/{estimate_id}` |
| Mark an estimate as sent | POST | `/estimates/{estimate_id}/status/sent` |
| Mark an estimate as accepted | POST | `/estimates/{estimate_id}/status/accepted` |
| Mark an estimate as declined | POST | `/estimates/{estimate_id}/status/declined` |
| Email an estimate | POST | `/estimates/{estimate_id}/email` |
| Bulk export estimates | GET | `/estimates/pdf` |
| Bulk print estimates | GET | `/estimates/print` |
| List estimate templates | GET | `/estimates/templates` |
| Add Comments | POST | `/estimates/{estimate_id}/comments` |

## Invoices — doc section: `invoices` ✅ fully confirmed, see references/invoice-example.md
| Action | Method | Path |
|---|---|---|
| Create an invoice | POST | `/invoices` |
| List invoices | GET | `/invoices` |
| Get an invoice | GET | `/invoices/{invoice_id}` |
| Update an invoice | PUT | `/invoices/{invoice_id}` |
| Delete an invoice | DELETE | `/invoices/{invoice_id}` |
| Update custom field in existing invoices | PUT | `/invoice/{invoice_id}/customfields` |
| Mark an invoice as sent | POST | `/invoices/{invoice_id}/status/sent` |
| Void an invoice | POST | `/invoices/{invoice_id}/status/void` |
| Mark as draft | POST | `/invoices/{invoice_id}/status/draft` |
| Email an invoice | POST | `/invoices/{invoice_id}/email` |
| Get invoice email content | GET | `/invoices/{invoice_id}/email` |
| Email invoices (bulk) | POST | `/invoices/email` |
| Remind Customer | POST | `/invoices/{invoice_id}/paymentreminder` |
| Bulk invoice reminder | POST | `/invoices/paymentreminder` |
| Bulk export Invoices (PDF) | GET | `/invoices/pdf` |
| Bulk print invoices | GET | `/invoices/print` |
| Disable/Enable payment reminder | POST | `/invoices/{invoice_id}/paymentreminder/disable` or `/enable` |
| Write off invoice | POST | `/invoices/{invoice_id}/writeoff` |
| Cancel write off | POST | `/invoices/{invoice_id}/writeoff/cancel` |
| Update billing/shipping address | PUT | `/invoices/{invoice_id}/address/billing` or `/shipping` |
| List invoice templates | GET | `/invoices/templates` |
| Update invoice template | PUT | `/invoices/{invoice_id}/templates/{template_id}` |
| List invoice payments | GET | `/invoices/{invoice_id}/payments` |
| List/apply/delete credits applied | GET/POST/DELETE | `/invoices/{invoice_id}/creditsapplied`, `/credits`, `/creditsapplied/{creditnotes_invoice_id}` |
| Delete a payment | DELETE | `/invoices/{invoice_id}/payments/{invoice_payment_id}` |
| Attachments | POST/PUT/GET/DELETE | `/invoices/{invoice_id}/attachment` |
| Comments | POST/GET/PUT/DELETE | `/invoices/{invoice_id}/comments[/{comment_id}]` |

## Recurring Invoices — doc section: `recurring-invoices`
| Action | Method | Path |
|---|---|---|
| Create a Recurring Invoice | POST | `/recurringinvoices` |
| List Recurring Invoices | GET | `/recurringinvoices` |
| Get / Update / Delete | GET/PUT/DELETE | `/recurringinvoices/{recurring_invoice_id}` |
| Stop | POST | `/recurringinvoices/{recurring_invoice_id}/status/stop` |
| Resume | POST | `/recurringinvoices/{recurring_invoice_id}/status/resume` |
| List Recurring Invoice History | GET | `/recurringinvoices/{recurring_invoice_id}/history` |

## Customer Payments — doc section: `customer-payments`
| Action | Method | Path |
|---|---|---|
| Create a payment | POST | `/customerpayments` |
| List Customer Payments | GET | `/customerpayments` |
| Retrieve / Update / Delete a payment | GET/PUT/DELETE | `/customerpayments/{payment_id}` |
| Refund an excess customer payment | POST | `/customerpayments/{payment_id}/refunds` |
| List/Update/Get/Delete refunds | GET/PUT/GET/DELETE | `/customerpayments/{payment_id}/refunds[/{refund_id}]` |

## Retainer Invoices — doc section: `retainer-invoices`
| Action | Method | Path |
|---|---|---|
| Create | POST | `/retainerinvoices` |
| List | GET | `/retainerinvoices` |
| Get / Update / Delete | GET/PUT/DELETE | `/retainerinvoices/{retainerinvoice_id}` |
| Mark as sent | POST | `/retainerinvoices/{retainerinvoice_id}/status/sent` |
| Void | POST | `/retainerinvoices/{retainerinvoice_id}/status/void` |
| Mark as draft | POST | `/retainerinvoices/{retainerinvoice_id}/status/draft` |
| Email | POST | `/retainerinvoices/{retainerinvoice_id}/email` |

## Credit Notes — doc section: `credit-notes`
| Action | Method | Path |
|---|---|---|
| Create | POST | `/creditnotes` |
| List | GET | `/creditnotes` |
| Get / Update / Delete | GET/PUT/DELETE | `/creditnotes/{creditnote_id}` |
| Email | POST | `/creditnotes/{creditnote_id}/email` |
| Void | POST | `/creditnotes/{creditnote_id}/status/void` |
| Open a voided credit note | POST | `/creditnotes/{creditnote_id}/status/open` |
| Credit to an invoice | POST | `/creditnotes/{creditnote_id}/invoices` |
| List/Delete invoices credited | GET/DELETE | `/creditnotes/{creditnote_id}/invoices[/{invoice_id}]` |
| Refunds | GET/POST/PUT/DELETE | `/creditnotes/{creditnote_id}/refunds[/{refund_id}]` |

## Expenses — doc section: `expenses`
| Action | Method | Path |
|---|---|---|
| Create an Expense | POST | `/expenses` |
| List Expenses | GET | `/expenses` |
| Get / Update / Delete | GET/PUT/DELETE | `/expenses/{expense_id}` |
| List expense History & Comments | GET | `/expenses/{expense_id}/comments` |
| Create/List/Delete employee | POST/GET/DELETE | `/employees[/{employee_id}]` |

## Recurring Expenses — doc section: `recurring-expenses`
| Action | Method | Path |
|---|---|---|
| Create | POST | `/recurringexpenses` |
| List | GET | `/recurringexpenses` |
| Get / Update / Delete | GET/PUT/DELETE | `/recurringexpenses/{recurring_expense_id}` |
| Stop / Resume | POST | `/recurringexpenses/{id}/status/stop` or `/resume` |
| List child expenses created | GET | `/recurringexpenses/{id}/expenses` |
| List history | GET | `/recurringexpenses/{id}/history` |

## Projects — doc section: `projects`
| Action | Method | Path |
|---|---|---|
| Create a project | POST | `/projects` |
| List projects | GET | `/projects` |
| Get / Update / Delete | GET/PUT/DELETE | `/projects/{project_id}` |
| Activate / Deactivate | POST | `/projects/{project_id}/active` or `/inactive` |
| Clone | POST | `/projects/{project_id}/clone` |
| Users (assign/list/invite/update/get/delete) | various | `/projects/{project_id}/users[/{user_id}]` |
| Comments | POST/GET/DELETE | `/projects/{project_id}/comments[/{comment_id}]` |
| List invoices for project | GET | `/projects/{project_id}/invoices` |
| Tasks (add/list/update/get/delete) | various | `/projects/{project_id}/tasks[/{task_id}]` |

## Time Entries — doc section: `time-entries`
| Action | Method | Path |
|---|---|---|
| Log time entries | POST | `/timeentries` |
| List time entries | GET | `/timeentries` |
| Get / Update / Delete | GET/PUT/DELETE | `/timeentries/{time_entry_id}` |
| Start / Stop / Get timer | POST/POST/GET | `/timeentries/timer/start`, `/timer/stop`, `/timer` |

## Users — doc section: `users`
| Action | Method | Path |
|---|---|---|
| Create a user | POST | `/users` |
| List Users | GET | `/users` |
| Get / Update / Delete | GET/PUT/DELETE | `/users/{user_id}` |
| Get current user | GET | `/users/me` |
| Invite a user | POST | `/users/invite` |
| Mark active / inactive | POST | `/users/{user_id}/active` or `/inactive` |

## Taxes — doc section: `taxes`
| Action | Method | Path |
|---|---|---|
| Create / List a tax | POST/GET | `/settings/taxes` |
| Get / Update / Delete a tax | GET/PUT/DELETE | `/settings/taxes/{tax_id}` |
| Tax groups | POST/GET/PUT/DELETE | `/settings/taxgroups[/{tax_group_id}]` |
| Tax exemptions | POST/GET/PUT/DELETE | `/settings/taxexemptions[/{tax_exemption_id}]` |
| Tax authorities | POST/GET/PUT/DELETE | `/settings/taxauthorities[/{tax_authority_id}]` |

## Expense Category — doc section: `expense-category`
| Action | Method | Path |
|---|---|---|
| Create / List | POST/GET | `/expensecategories` |
| Get / Update / Delete | GET/PUT/DELETE | `/expensecategories/{expense_category_id}` |
| Mark active / inactive | POST | `/expensecategories/{id}/active` or `/inactive` |

## Currency — doc section: `currency`
| Action | Method | Path |
|---|---|---|
| Create / List a currency | POST/GET | `/settings/currencies` |
| Get / Update / Delete | GET/PUT/DELETE | `/settings/currencies/{currency_id}` |
| Exchange rates | POST/GET/PUT/DELETE | `/settings/currencies/{currency_id}/exchangerates[/{exchange_rate_id}]` |

## Zoho CRM Integration — doc section: `integration`
| Action | Method | Path |
|---|---|---|
| Import customer using CRM account ID | POST | `/contacts/crm/account/{crm_account_id}` |
| Import customer using CRM contact ID | POST | `/contacts/crm/contact/{crm_contact_id}` |
| Import item using CRM product ID | POST | `/items/crm/product/{crm_product_id}` |

---
**If any path above 404s**, fetch the live section page (`https://www.zoho.com/invoice/api/v3/{doc-section}/`) or that resource's OpenAPI yml and correct this table — the doc-section names in each heading above are exact.