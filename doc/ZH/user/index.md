---
description: 'Read through the GitLab User documentation to learn how to use, configure, and customize GitLab and GitLab.com to your own needs.'
---

# 用户文档

欢迎来到GitLab !我们很高兴你能来!

您将可以访问订阅包含的所有[功能](https://about.gitlab.com/pricing/)，[GitLab管理员](../administration/index.md)设置除外(除非您有安装、配置和升级您的GitLab实例的管理特权。)

[GitLab.com](https://gitlab.com/)的管理权限仅限于GitLab团队。

有关配置GitLab自管理实例的更多信息，请参见[管理员文档](../administration/index.md)。


## 概述

GitLab是一个完全集成的软件开发平台，它使您的团队能够透明、快速、有效和有凝聚力，从讨论新想法到生产，都在同一个平台上。

有关更多信息，请参见[所有GitLab特性](https://about.gitlab.com/features/)。


### 概念

为了熟悉在GitLab上开发代码所需的概念，请阅读以下文章:

- [演示: 掌握代码审查(Code Review)与GitLab](https://about.gitlab.com/blog/2017/03/17/demo-mastering-code-review-with-gitlab/).
- [GitLab工作流: 概述](https://about.gitlab.com/blog/2016/10/25/gitlab-workflow-an-overview/#gitlab-workflow-use-case-scenario).
- [Tutorial: 它都是在GitLab中连接的](https://about.gitlab.com/blog/2016/03/08/gitlab-tutorial-its-all-connected/): 关于与GitLab的代码协作的概述。
- [版本控制领域的趋势:微服务](https://about.gitlab.com/blog/2016/08/16/trends-in-version-control-land-microservices/).
- [版本控制领域的趋势:内包](https://about.gitlab.com/blog/2016/07/07/trends-version-control-innersourcing/).

## 用例

GitLab是一个基于Git的平台，它集成了大量用于软件开发和部署以及项目管理的基本工具:

- 托管代码与版本控制存储库。
- 跟踪新建议的实现、bug报告和反馈, 使用功能齐全的[问题跟踪器](project/issues/index.md#issues-list)。
- 通过[看板](project/issues/index.md#issue-boards)进行组织和优先级管理
- 使用[审查应用程序](../ci/review_apps/index.md)审查[合并请求](project/merge_requests/index.md)中的代码，实时预览每个分支的更改。
- 使用内置的[持续集成(CI)](../ci/README.md)进行构建、测试和部署。
- 使用[GitLab Pages](project/pages/index.md)部署个人和专业的静态网站。
- 通过使用[GitLab Container Registry](packages/container_registry/index.md)与Docker集成。
- 使用[GitLab Cycle Analytics](project/cycle_analytics.md)跟踪开发生命周期。

通过GitLab企业版，您还可以：

- 提供[服务台](project/service_desk.md)支持。
- 利用下面功能, 提高协作:
  - [Merge Request Approvals](project/merge_requests/merge_request_approvals.md). **(STARTER)**
  - [Multiple Assignees for Issues](project/issues/multiple_assignees_for_issues.md). **(STARTER)**
  - [Multiple Issue Boards](project/issue_board.md#multiple-issue-boards).
- 在issues与[相关issues](project/issues/related_issues.md)之间建立正式的关系.
- 使用[燃尽图](project/milestones/burndown_charts.md)来跟踪在sprint期间或开发新版本软件时的进度。
- [Elasticsearch](../integration/elasticsearch.md)具有[高级全局搜索](search/advanced_global_search.md)和[高级语法搜索](search/advanced_search_syntax.md)，利用它可以在整个GitLab实例上进行更快、更高级的代码搜索。
- [使用Kerberos对用户进行身份验证](../integration/kerberos.md).
- [Mirror a repository](project/repository/repository_mirroring.md) from elsewhere on your local server.
- [导出issues为CSV](project/issues/csv_export.md).
- 查看整个CI/CD管道涉及多个项目和[多个项目管道](../ci/multi_project_pipeline_graphs.md)。
- [锁定文件](project/file_lock.md)以防止冲突
- 使用[部署板](project/deploy_boards.md)查看Kubernetes上运行的每个CI环境的当前健康状况和状态。
- 使用[Canary Deployments](project/canary_deployments.md)进行持续交付
- 扫描您的代码寻找漏洞，并[在合并请求中显示它们](application_security/sast/index.md)。

您还可以将GitLab与许多第三方应用程序[集成](project/integrations/project_services.md)，如 Mattermost, Microsoft Teams, HipChat, Trello, Slack, Bamboo CI, Jira, 等.

## Projects(项目)

在GitLab中，您可以创建[projects](project/index.md)来托管您的代码、跟踪问题、在代码上进行协作，并使用内置的GitLab CI/CD不断地构建、测试和部署您的应用程序。或者，您可以一次完成所有工作，从一个项目开始。

- [Repositories](project/repository/index.md): 将代码库驻留在带有版本控制的存储库中，并将其作为完整集成平台的一部分。
- [Issues(议题)](project/issues/index.md): 探索GitLab Issues的最佳特性。
- [Merge Requests(合并请求)](project/merge_requests/index.md): l利用合并请求，在代码、评审、实时预览每个分支的更改以及请求批准方面进行协作
- [Milestones(里程碑)](project/milestones/index.md): 处理多个问题，并将同一目标日期的请求与里程碑合并。

## GitLab CI/CD

使用内置的 [GitLab CI/CD](../ci/README.md) 直接从GitLab测试、构建和部署应用程序。不需要第三方集成。

- [GitLab Auto Deploy](../topics/autodevops/index.md#auto-deploy): 使用GitLab自动部署来开箱即用地部署您的应用程序。
- [Review Apps](../ci/review_apps/index.md): 实时预览与审查应用程序合并请求引入的更改。
- [GitLab Pages](project/pages/index.md): 使用GitLab Pages直接从GitLab发布静态站点。您可以使用页面构建、测试和部署任何静态站点生成器。
- [GitLab Container Registry](packages/container_registry/index.md): 使用容器注册表构建和部署Docker映像。

## Account(账户)

您可以定制和配置许多资源来享受GitLab的最佳效果。

- [Settings](profile/index.md): 管理您的用户设置来更改您的个人信息、个人访问令牌、授权应用程序等。
- [Authentication](../topics/authentication/index.md): 阅读GitLab中可用的身份验证方法。
- [Permissions](permissions.md): 了解每个用户类型(来宾、报告者、开发人员、维护人员、所有者)的不同权限级别集。
- [Feature highlight](feature_highlight.md): Learn 了解更多关于小蓝点周围的应用程序，解释某些功能。
- [Abuse reports](abuse_reports.md): 向GitLab管理员报告用户的滥用情况。

## Groups (群组)

使用GitLab [Groups](group/index.md)，您可以将相关的项目组合在一起，并授予成员一次访问多个项目的权限。

群组也可以嵌套在[子群组](group/subgroups/index.md)中。

## Discussions

在GitLab中，您可以在issues,
merge requests, code snippets 和 commits中注释和提到协作者。

当通过merge requests对代码库的实现执行内联审查时，您可以通过[可解析线程](discussions/index.md#resolvable-comments-and-threads)收集反馈。

### GitLab Flavored Markdown (GFM)

请阅读[GFM文档](markdown.md)，了解如何在您的线程、注释、问题和合并请求描述以及支持GFM的其他地方应用GitLab风格的Markdown。

## Todos

永远不要忘记回复你的合作者。[GitLab Todos](todos.md)是一个工具，可以更快、更有效地与您的团队一起工作，它可以列出所有用户或组提到的内容，以及分配给您的issues 和 merge
requests。

## Search

通过groups、projects、issues、merge requests、文件、代码等进行[搜索和筛选](search/index.md)。

## Snippets(代码片段)

[Snippets](snippets.md)是您希望存储在GitLab中的代码块，您可以快速访问这些代码块。你也可以通过[Discussions](#Discussions)收集反馈。

## 键盘快捷键

GitLab中有许多[键盘快捷键](shortcuts.md)，帮助您在页面之间导航，更快地完成任务。

## Integrations

将GitLab与您喜欢的工具[集成](../integration/README.md)，如Trello、Jira等。

## Webhooks

配置[webhooks](project/integrations/webhooks.md)以侦听特定事件，如pushes, issue或merge requests。GitLab将发送一个带有数据的POST请求到webhook URL。

## API

通过 [API](../api/README.md), 自动操作 GitLab.

## Git and GitLab

了解[Git](../topics/git/index.md)及其最佳实践。

## 实例数据

查看GitLab实例的[各种统计数据](instance_statistics/index.md)。

## 操作仪表板 **(PREMIUM)**

有关每个项目的运行状况的摘要，请参见[操作仪表板](operations_dashboard/index.md)。