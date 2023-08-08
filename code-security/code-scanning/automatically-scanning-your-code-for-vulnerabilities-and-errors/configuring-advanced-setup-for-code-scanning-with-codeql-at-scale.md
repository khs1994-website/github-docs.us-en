---
title: Configuring {% ifversion code-scanning-without-workflow %}advanced setup for {% endif %}code scanning with CodeQL at scale
shortTitle: CodeQL {% ifversion code-scanning-without-workflow %}advanced setup{% else %}code scanning{% endif %} at scale
intro: 'You can use a script to configure advanced setup for {% data variables.product.prodname_code_scanning %} for a specific group of repositories in your organization.'
product: '{% data reusables.gated-features.code-scanning %}'
versions:
  fpt: '*'
  ghec: '*'
  ghes: '*'
  ghae: '*'
type: how_to
topics:
  - Advanced Security
  - Code scanning
allowTitleToDifferFromFilename: true
---

## About configuring advanced setup for {% data variables.product.prodname_code_scanning %} with {% data variables.product.prodname_codeql %} at scale

If you need to configure a highly customizable {% data variables.product.prodname_code_scanning %} setup for many repositories in your organization, or if repositories in your organization are ineligible for default setup, you can configure {% data variables.product.prodname_code_scanning %} at scale with advanced setup.

To configure advanced setup across multiple repositories, you can write a bulk configuration script. To successfully execute the script, {% data variables.product.prodname_actions %} must be enabled for the {% ifversion fpt %}organization{% elsif ghec or ghae %}organization or enterprise{% elsif ghes %}site{% endif %}.

{% ifversion code-scanning-without-workflow %}
Alternatively, if you do not need granular control over the {% data variables.product.prodname_code_scanning %} configuration for many repositories in your organization, you can quickly and easily configure {% data variables.product.prodname_code_scanning %} at scale with default setup. For more information, see "[AUTOTITLE](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-default-setup-for-code-scanning-at-scale)."
{% endif %}

## Using a script to configure advanced setup

For repositories that are not eligible for default setup, you can use a bulk configuration script to configure advanced setup across multiple repositories.

1. Identify a group of repositories that can be analyzed using the same {% data variables.product.prodname_code_scanning %} configuration. For example, all repositories that build Java artifacts using the production environment.
2. Create and test a {% data variables.product.prodname_actions %} workflow to call the {% data variables.product.prodname_codeql %} action with the appropriate configuration. For more information, see {% ifversion code-scanning-without-workflow %}"[AUTOTITLE](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-advanced-setup-for-code-scanning#configuring-advanced-setup-for-code-scanning-with-codeql)."{% else %}"[AUTOTITLE](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-advanced-setup-for-code-scanning#configuring-code-scanning-using-the-codeql-action)."{% endif %}
3. Use one of the example scripts create a custom script to add the workflow to each repository in the group.
   - PowerShell example: [`jhutchings1/Create-ActionsPRs`](https://github.com/jhutchings1/Create-ActionsPRs) repository
   - NodeJS example: [`nickliffen/ghas-enablement`](https://github.com/NickLiffen/ghas-enablement) repository