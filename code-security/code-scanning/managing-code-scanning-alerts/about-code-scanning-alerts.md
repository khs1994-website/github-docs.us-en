---
title: About code scanning alerts
intro: Learn about the different types of code scanning alerts and the information that helps you understand the problem each alert highlights.
product: '{% data reusables.gated-features.code-scanning %}'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
redirect_from:
  - /code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/about-code-scanning-alerts
type: overview
topics:
  - Advanced Security
  - Code scanning
  - CodeQL
---

{% data reusables.code-scanning.beta %}
{% data reusables.code-scanning.enterprise-enable-code-scanning %}

## About alerts from {% data variables.product.prodname_code_scanning %}

You can configure {% data variables.product.prodname_code_scanning %} to check the code in a repository using the default {% data variables.product.prodname_codeql %} analysis, a third-party analysis, or multiple types of analysis. When the analysis is complete, the resulting alerts are displayed alongside each other in the security view of the repository. Results from third-party tools or from custom queries may not include all of the properties that you see for alerts detected by {% data variables.product.company_short %}'s default {% data variables.product.prodname_codeql %} analysis. For more information, see {% ifversion code-scanning-without-workflow %}"[AUTOTITLE](/code-security/code-scanning/enabling-code-scanning/configuring-default-setup-for-code-scanning)" and {% endif %}"[AUTOTITLE](/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/configuring-advanced-setup-for-code-scanning)."

By default, {% data variables.product.prodname_code_scanning %} analyzes your code periodically on the default branch and during pull requests. For information about managing alerts on a pull request, see "[AUTOTITLE](/code-security/code-scanning/managing-code-scanning-alerts/triaging-code-scanning-alerts-in-pull-requests)."

{% data reusables.code-scanning.audit-code-scanning-events %}

## About alert details

Each alert highlights a problem with the code and the name of the tool that identified it. You can see the line of code that triggered the alert, as well as properties of the alert, such as the alert severity, security severity, and the nature of the problem. Alerts also tell you when the issue was first introduced. For alerts identified by {% data variables.product.prodname_codeql %} analysis, you will also see information on how to fix the problem.

{% data reusables.code-scanning.alert-default-branch %}

{% ifversion fpt or ghec or ghes > 3.8 %}
![Screenshot showing the elements of a {% data variables.product.prodname_code_scanning %} alert, including the title of the alert and relevant lines of code at left and the severity level, affected branches, and weaknesses at right. ](/assets/images/help/repository/code-scanning-alert.png)
{% else %}
![Screenshot showing the elements of a {% data variables.product.prodname_code_scanning %} alert, including the title of the alert and relevant lines of code at left and the severity level, affected branches, and weaknesses at right.](/assets/images/enterprise/code-security/code-scanning-alert.png)
{% endif %}
If you configure {% data variables.product.prodname_code_scanning %} using {% data variables.product.prodname_codeql %}, you can also find data-flow problems in your code. Data-flow analysis finds potential security issues in code, such as: using data insecurely, passing dangerous arguments to functions, and leaking sensitive information.

When {% data variables.product.prodname_code_scanning %} reports data-flow alerts, {% data variables.product.prodname_dotcom %} shows you how data moves through the code. {% data variables.product.prodname_code_scanning_caps %} allows you to identify the areas of your code that leak sensitive information, and that could be the entry point for attacks by malicious users.

### About severity levels

Alert severity levels may be `Error`, `Warning`, or `Note`.

If {% data variables.product.prodname_code_scanning %} is enabled as a pull request check, the check will fail if it detects any results with a severity of `error`. You can specify which severity level of code scanning alerts causes a check failure. For more information, see "[AUTOTITLE](/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/customizing-your-advanced-setup-for-code-scanning#defining-the-severities-causing-pull-request-check-failure)."

### About security severity levels

{% data variables.product.prodname_code_scanning_caps %} displays security severity levels for alerts that are generated by security queries. Security severity levels can be `Critical`, `High`, `Medium`, or `Low`.

To calculate the security severity of an alert, we use Common Vulnerability Scoring System (CVSS) data. CVSS is an open framework for communicating the characteristics and severity of software vulnerabilities, and is commonly used by other security products to score alerts. For more information about how severity levels are calculated, see [this blog post](https://github.blog/changelog/2021-07-19-codeql-code-scanning-new-severity-levels-for-security-alerts/).

By default, any {% data variables.product.prodname_code_scanning %} results with a security severity of `Critical` or `High` will cause a check failure. You can specify which security severity level for {% data variables.product.prodname_code_scanning %} results should cause a check failure. For more information, see "[AUTOTITLE](/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/customizing-your-advanced-setup-for-code-scanning#defining-the-severities-causing-pull-request-check-failure)."

### About {% ifversion remove-code-scanning-configurations %}alerts from multiple configurations{% else %}analysis origins{% endif %}

{% ifversion remove-code-scanning-configurations %}
You can run multiple configurations of code analysis on a repository, using different tools and targeting different languages or areas of the code. Each configuration of {% data variables.product.prodname_code_scanning %} generates a unique set of alerts. For example, an alert generated using the default {% data variables.product.prodname_codeql %} analysis with {% data variables.product.prodname_actions %} comes from a different configuration than an alert generated externally and uploaded via the {% data variables.product.prodname_code_scanning %} API.

If you use multiple configurations to analyze a file, any problems detected by the same query are reported as alerts generated by multiple configurations. If an alert exists in more than one configuration, the number of configurations appears next to the branch name in the "Affected branches" section on the right-hand side of the alert page. To view the configurations for an alert, in the "Affected branches" section, click a branch. A "Configurations analyzing" modal appears with the names of each configuration generating the alert for that branch. Below each configuration, you can see when that configuration's alert was last updated.

An alert may display different statuses from different configurations. To update the alert statuses, re-run each out-of-date configuration. Alternatively, you can delete stale configurations from a branch to remove outdated alerts. For more information on deleting stale configurations and alerts, see "[AUTOTITLE](/code-security/code-scanning/managing-code-scanning-alerts/managing-code-scanning-alerts-for-your-repository#removing-stale-configurations-and-alerts-from-a-branch)."
{% else %}
You can run multiple configurations of code analysis on a repository, using different tools and targeting different languages or areas of the code. Each configuration of {% data variables.product.prodname_code_scanning %} is the analysis origin for all the alerts it generates. For example, an alert generated using the default {% data variables.product.prodname_codeql %} analysis with {% data variables.product.prodname_actions %} will have a different analysis origin from an alert generated externally and uploaded via the {% data variables.product.prodname_code_scanning %} API.

If you use multiple configurations to analyze a file, any problems detected by the same query are reported as alerts with multiple analysis origins. If an alert has more than one analysis origin, a {% octicon "workflow" aria-label="The workflow icon" %} icon will appear next to any relevant branch in the **Affected branches** section on the right-hand side of the alert page. You can hover over the {% octicon "workflow" aria-label="The workflow icon" %} icon to see the names of each analysis origin and the status of the alert for that analysis origin. You can also view the history of when alerts appeared in each analysis origin in the timeline on the alert page. If an alert only has one analysis origin, no information about analysis origins is displayed on the alert page.

![Screenshot showing a code scanning alert with multiple analysis origins.](/assets/images/help/repository/code-scanning-analysis-origins.png)

{% note %}

**Note:** Sometimes a {% data variables.product.prodname_code_scanning %} alert displays as fixed for one analysis origin but is still open for a second analysis origin. You can resolve this by re-running the second {% data variables.product.prodname_code_scanning %} configuration to update the alert status for that analysis origin.

{% endnote %}
{% endif %}

### About labels for alerts that are not found in application code

{% data variables.product.product_name %} assigns a category label to alerts that are not found in application code. The label relates to the location of the alert.

- Generated: Code generated by the build process
- Test: Test code
- Library: Library or third-party code
- Documentation: Documentation

{% data variables.product.prodname_code_scanning_caps %} categorizes files by file path. You cannot manually categorize source files.

In this example, an alert is marked as in "Test" code in the {% data variables.product.prodname_code_scanning %} alert list.

![Screenshot of an alert in the {% data variables.product.prodname_code_scanning %} list. To the right of the title, a "Test" label is highlighted with a dark orange outline.](/assets/images/help/repository/code-scanning-library-alert-index.png)

When you click through to see details for the alert, you can see that the file path is marked as "Test" code.

![Screenshot showing the details of an alert. The file path and "Test" label are highlighted with a dark orange outline.](/assets/images/help/repository/code-scanning-library-alert-show.png)

{% ifversion codeql-ml-queries %}

## About experimental alerts

{% data reusables.code-scanning.beta-codeql-ml-queries %}

In repositories that run {% data variables.product.prodname_code_scanning %} using the {% data variables.product.prodname_codeql %} action, you may see some alerts that are marked as experimental. These are alerts that were found using a machine learning model to extend the capabilities of an existing {% data variables.product.prodname_codeql %} query.

![Screenshot showing an alert for {% data variables.product.prodname_code_scanning %}. An "Experimental" label is displayed to the right of the title, which is appended with "(experimental)."](/assets/images/help/repository/code-scanning-experimental-alert-list.png)

### Benefits of using machine learning models to extend queries

Queries that use machine learning models are capable of finding vulnerabilities in code that was written using frameworks and libraries that the original query writer did not include.

Each of the security queries for {% data variables.product.prodname_codeql %} identifies code that's vulnerable to a specific type of attack. Security researchers write the queries and include the most common frameworks and libraries. So each existing query finds vulnerable uses of common frameworks and libraries. However, developers use many different frameworks and libraries, and a manually maintained query cannot include them all. Consequently, manually maintained queries do not provide coverage for all frameworks and libraries.

{% data variables.product.prodname_codeql %} uses a machine learning model to extend an existing security query to cover a wider range of frameworks and libraries. The machine learning model is trained to detect problems in code it's never seen before. Queries that use the model will find results for frameworks and libraries that are not described in the original query.

### Alerts identified using machine learning

Alerts found using a machine learning model are displayed with an "Experimental alerts" banner to show that the technology is under active development. These alerts have a higher rate of false positive results than the queries they are based on. The machine learning model will improve based on user actions such as marking a poor result as a false positive or fixing a good result.

## Enabling experimental alerts

The default {% data variables.product.prodname_codeql %} query suites do not include any queries that use machine learning to generate experimental alerts. To run machine learning queries during {% data variables.product.prodname_code_scanning %} you need to run the additional queries contained in one of the following query suites.

{% data reusables.code-scanning.codeql-query-suites %}

When you update your workflow to run an additional query suite this will increase the analysis time.

``` yaml
- uses: {% data reusables.actions.action-codeql-action-init %}
  with:
    # Run extended queries including queries using machine learning
    queries: security-extended
```

For more information, see "[AUTOTITLE](/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/customizing-your-advanced-setup-for-code-scanning#using-queries-in-ql-packs)."

## Disabling experimental alerts

The simplest way to disable queries that use machine learning to generate experimental alerts is to stop running the `security-extended` or `security-and-quality` query suite. In the example above, you would comment out the `queries` line. If you need to continue to run the `security-extended` or `security-and-quality` suite and the machine learning queries are causing problems, then you can open a ticket with [{% data variables.product.company_short %} support](https://support.github.com/contact) with the following details.

- Ticket title: "{% data variables.product.prodname_code_scanning %}: removal from experimental alerts beta"
- Specify details of the repositories or organizations that are affected
- Request an escalation to engineering

{% endif %}