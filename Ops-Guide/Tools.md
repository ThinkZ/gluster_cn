# # 我们使用的工具

|服务/工具 |Purpose                            |在托管 |

|----------------------|------------------------------------|-----------------|
|Gerrit |代码审查 | 临时架 |

|詹金斯 |CI，生成-验证-测试 | 临时架 |

|备份 |网站，Gerrit 和詹金斯备份 | 临时架 |

|虫子 WebUI |所有打开的 bug 的仪表板 | 临时架 |

|Docs                 |文档内容 |readthedocs.org |

|download.gluster.org |二进制文件的官方下载网站 |临时架 |

|声纳 |静态分析 |Rackspace 公司 |

|盐-大师 |管理的一部分红 |Rackspace 公司 |

|Web 生成器 |构建和部署网站 Cronjob |Rackspace 公司 |

|邮递员 |邮递员列表 |Rackspace 公司 |

|www.gluster.org |Web 资产 |Rackspace 公司 |

|该群 |真实姓名的网站服务器 （和做 it 所有服务器） |Rackspace 公司 |


# # 笔记
* download.gluster.org︰ 弹性是重要的可用性和度量。
因为它是官方下载，访问需要限制，尽可能多地。
TODO︰ 谁有权访问？
* 声纳︰ 常用并大多处于空闲状态。
* 盐大师︰ 迈克尔会有更多的细节。TODO︰ 在此添加更多详细信息。

* Web-生成器︰ 由 misc 与盐。无状态的可以会丢弃和

重新安装。
* 邮递员︰ 应该迁移到单独的主机。应将更多冗余

(即，超过 1 MX)。
* www.gluster.org︰ 工件的框架，现在存在下 gluster.github.com。有

各种旧式安装的软件 （mediawiki 等），作为正在打扫
我们找到他们。
