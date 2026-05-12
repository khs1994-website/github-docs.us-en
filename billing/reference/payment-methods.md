---
title: Payment methods
shortTitle: Payment methods
intro: 'Reference information for supported payment methods for {% data variables.product.github %}.'
versions:
  fpt: '*'
  ghec: '*'
  ghes: '*'
contentType: reference
category:
  - Set up payment
redirect_from:
  - /billing/reference/supported-payment-methods
---

The payment methods available to you depend on your account type and the products you use.

## Self-serve accounts

Self-serve accounts can use the following payment methods:

* Credit card
* PayPal
* Azure Subscription ID (not personal accounts)

## Invoiced accounts

Invoiced accounts can use different payment methods for volume-licensed products (such as {% data variables.product.prodname_ghe_cloud %} and {% data variables.product.prodname_GH_advanced_security %}) and metered products (such as {% data variables.product.prodname_github_codespaces %}). You can combine payment methods for different product types. For example, you can use invoice for volume products and Azure Subscription ID for metered products.

| Payment method | Volume products | Metered products |
|---|---|---|
| Credit card | {% octicon "x" aria-label="Not supported" %} | {% octicon "check" aria-label="Supported" %} |
| PayPal | {% octicon "x" aria-label="Not supported" %} | {% octicon "check" aria-label="Supported" %} |
| Azure Subscription ID | {% octicon "x" aria-label="Not supported" %} | {% octicon "check" aria-label="Supported" %} |
| Invoice | {% octicon "check" aria-label="Supported" %} | {% octicon "check" aria-label="Supported" %} |
| Prepaid credits | {% octicon "check" aria-label="Supported" %} | {% octicon "x" aria-label="Not supported" %} |
| Purchase order | {% octicon "check" aria-label="Supported" %} | {% octicon "check" aria-label="Supported" %} |
| ACH | {% octicon "x" aria-label="Not supported" %} | {% octicon "check" aria-label="Supported" %} |

> [!NOTE]
> For purchase orders, contact [{% data variables.product.github %}'s Sales team](https://github.com/enterprise/contact?scid=&utm_campaign=2023q3-site-ww-PremiumSupport&utm_content=Premium+Support&utm_medium=referral&utm_source=github).
