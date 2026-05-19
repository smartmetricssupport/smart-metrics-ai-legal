# Privacy Policy — Smart Metrics AI

**Last updated:** May 2026

This Privacy Policy describes how Smart Metrics AI ("the App", "we", "our") collects, uses, and protects information when you use the App within your Atlassian Jira Cloud instance.

---

## 1. Who We Are

Smart Metrics AI is an Atlassian Marketplace app built on the Atlassian Forge platform. The App is operated independently and is not affiliated with or endorsed by Atlassian Pty Ltd.

---

## 2. Data We Collect

Smart Metrics AI collects and processes the following data **solely within your Atlassian tenant**:

### 2.1 Jira Issue Data
When you configure a project, the App reads and caches the following fields from Jira issues via the Jira REST API:
- Issue key, summary, and type
- Issue status and status transition history (changelog)
- Creation date and resolution date
- Story Points (custom field)
- Parent issue key (for sub-task relationships)
- Issue labels
- Assignee display name

This data is used exclusively to calculate engineering metrics (Lead Time, Cycle Time, Throughput, Friction Index, etc.).

### 2.2 Project Configuration
The App stores configuration data you enter in the Admin panel:
- Project key
- Mapped status names (Done, Active)
- Team member names (optional)
- Cycle Time strategy preference

### 2.3 Sync Metadata
The App stores timestamps of the last successful sync operation per project.

---

## 3. Where Data Is Stored

**All data is stored exclusively in Atlassian Forge Entity Storage and Key-Value Store (KVS)**, which is:
- Scoped to your specific Atlassian site
- Hosted and managed by Atlassian on their infrastructure
- Never accessible to us or any third party outside your tenant

The App makes **no calls to any external server or third-party service**. All processing occurs within the Atlassian Forge runtime environment.

---

## 4. How Data Is Used

Collected data is used solely to:
- Calculate and display engineering metrics on the App dashboard
- Power predictive simulations (Monte Carlo)
- Identify flow patterns and bottlenecks (VSM)
- Detect at-risk issues in active sprints

The App does **not** use your data for any other purpose, including advertising, profiling, or training machine learning models.

---

## 5. Who Has Access

- **You and your team**: any Jira user with access to the App within your site can view the dashboard metrics.
- **Jira Admins**: can configure which projects are analyzed and manage settings.
- **We (the App developer)**: have no access to your Jira data. We cannot read, access, or export the issue data or configurations stored in your tenant.

---

## 6. Data Retention

Data is retained in Forge Storage as long as the App is installed on your site and you have not deleted a project configuration.

You can delete all stored data for a project at any time via **Apps → Smart Metrics AI → Configuration → Delete Project**.

When the App is uninstalled from your Jira site, Atlassian's Forge platform automatically deletes all associated App storage data.

---

## 7. Data Security

The App relies entirely on Atlassian Forge's security model:
- All storage is encrypted at rest by Atlassian.
- All communication between the App frontend and backend uses Atlassian's internal secure channels (no external HTTP calls).
- Access is controlled by Jira's own permission system.

For details on Atlassian's security practices, refer to [Atlassian's Trust Center](https://www.atlassian.com/trust).

---

## 8. Third-Party Services

Smart Metrics AI does **not** integrate with or transmit data to any third-party services.

The App may load fonts from Google Fonts CDN (`fonts.googleapis.com`, `fonts.gstatic.com`) for UI rendering. These are standard static font files and do not transmit any user or issue data.

---

## 9. Children's Privacy

The App is designed for professional use in software development teams. We do not knowingly collect information from children under 13 years of age.

---

## 10. International Data Transfers

Because all data is stored in Atlassian Forge Storage within your tenant's region, data transfers are governed by Atlassian's data residency policies. Please refer to [Atlassian's Data Residency documentation](https://support.atlassian.com/security-and-access-policies/docs/understand-data-residency/) for details.

---

## 11. Your Rights (GDPR / LGPD)

If you are located in the European Economic Area (EEA) or Brazil, you have rights under GDPR and LGPD respectively, including the right to access, correct, or delete your personal data. Since all data is stored within your own Atlassian tenant and we have no access to it, you can exercise these rights directly by:
- Deleting project configurations within the App
- Uninstalling the App (which removes all App storage)
- Contacting your Jira Administrator

---

## 12. Changes to This Policy

We may update this Privacy Policy from time to time. The "Last updated" date at the top of this document reflects the most recent revision. Continued use of the App after changes constitutes acceptance of the updated policy.

---

## 13. Contact

For privacy-related questions or concerns, please contact:

**Email:** [your-support-email@domain.com]
**GitHub:** [https://github.com/your-github-user/your-repo/issues](https://github.com/your-github-user/your-repo/issues)

---

*Smart Metrics AI is an independent app and is not affiliated with or endorsed by Atlassian Pty Ltd.*
