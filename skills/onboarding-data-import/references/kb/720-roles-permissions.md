# Roles & Permissions

Category: NeonAdmin · Article 720 · Updated 2026-02-25

Here is how roles & permissions work in NeonPanel, plus how to create custom roles

## Introduction

Roles and permissions are essential for implementing data security policies and measures. Properly configuring these dimensions will help you establish robust data security measures.

Let's recap what roles & permissions are from the NeonPanel Intro article.

## Permissions

By default, all pages, tabs, reports, and other content within NeonPanel are locked. Permissions are required to gain access to these various components, with two types of access available: ReadOnly and Edit. ReadOnly access allows you to view inventory orders (or specific tasks within them), whereas Edit grants you permission to change aspects of the relevant content.

## Roles

Roles are configured with specific permissions that are required for each individual worker to fulfill their job duties. According to the principle of least privilege, it is best to give each worker only the amount of access necessary - neither more nor less. For example, a data analyst may only need permission to view the analytics section of the app or even just a specific report.

Note that you can specify account-wide roles, which confer the rights of a role(s) across all companies belonging to an account. Alternatively, if you need more granular control, you can specify role(s) for each company.

Roles determine access, not users. Users can be assigned roles, and therefore inherit the access a role grants.

## Creating roles

Every business has a unique way of working, and roles will help you mimic your organization's business structure.

Although many roles are common across Amazon Seller businesses, you can create custom roles with granular permissions, if needed. To do so, navigate to Company Roles or Account Roles within NeonAdmin. The process of creating a custom role is similar in both cases.

Click on the Create Company Role/Create Account Role at the top right corner.

In the opened tab specify the Name of the role and the Company/Account within which this role will apply.

Next, select one or more permissions you want to assign to this custom role.

Each permission is represented as

access type : content

Meaning the user is assigned a custom role with permission view:catalogs will only have access to inspect the Catalogs section of the company/account and all the other content within NeonPanel will be unavailable for them.

Admin permissions enable access to NeonAdmin.

Once you have selected the permissions, scroll down and click Create Company Role/Create Account Role at the bottom right of the page.

Permissions themselves cannot be edited, deleted or added by users. If you need a permission not currently available, please email us at support@neonpanel.com.
