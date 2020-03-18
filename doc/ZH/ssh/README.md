---
type: howto, reference
---

# GitLab和SSH密钥

Git是一个分布式版本控制系统，这意味着您可以在本地工作，但也可以将更改共享或“推送”到其他服务器。在将更改推送到GitLab服务器之前，您需要一个用于共享信息的安全通信通道。

SSH协议提供了这种安全性，并允许您向GitLab远程服务器进行身份验证，而无需每次都提供用户名或密码。

有关SSH协议如何工作的更详细的解释，我们建议您阅读[DigitalOcean的这个不错的教程](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)。

## 要求

唯一的要求是在系统上安装OpenSSH客户端。 它已预安装在GNU / Linux和macOS上，但未预安装在Windows上。

根据Windows版本的不同，可以使用不同的方法来使用SSH密钥。

### Windows 10: Windows Linux子系统

从Windows 10开始，您可以[安装Linux的Windows子系统（WSL）](https://docs.microsoft.com/zh-CN/windows/wsl/install-win10),可以直接在Windows上运行Linux发行版，而无需增加虚拟机的开销。 安装和设置好后，您将可以使用Git和SSH客户端。

### Windows 10、8.1和7：适用于Windows的Git

在Windows 8.1和Windows 7上安装Git和SSH客户端的最简单方法是[Git for Windows](https://gitforwindows.org)。 它提供了一个Bash仿真（Git Bash），用于从命令行和`ssh-keygen`命令运行Git，这对于创建SSH密钥很有用，您将在下面学习到。

注意: **替代工具:**
尽管未在此页面中进行探讨，但是您可以使用一些替代工具。
[Cygwin](https://www.cygwin.com)是大量GNU和开放源代码工具的集合，它们提供的功能类似于Unix发行版。[PuttyGen](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)提供图形用户界面 [create SSH keys](https://tartarus.org/~simon/putty-snapshots/htmldoc/Chapter8.html#pubkey-puttygen).

## SSH密钥的类型以及可供选择的密钥

GitLab支持RSA，DSA，ECDSA和ED25519密钥。 它们的区别在于签名算法，其中一些具有其他优势。 有关更多信息，您可以阅读此内容[nice article on ArchWiki](https://wiki.archlinux.org/index.php/SSH_keys#Choosing_the_authentication_key_type)。我们将重点关注ED25519和RSA。

注意: **Note:**
作为管理员，您可以[限制允许使用的密钥及其最小长度](../security/ssh_keys_restrictions.md).默认情况下，所有密钥都是允许的，[GitLab.com](../user/gitlab_com/index.md#ssh-host-keys-fingerprints)也是如此。

### ED25519 SSH keys

按照[最佳做法](https://linux-audit.com/using-ed25519-openssh-keys-instead-of-dsa-rsa-ecdsa/)，您应该始终偏爱[ED25519](https://ed25519.cr.yp.to/) SSH密钥，因为它们比其他类型更安全并且具有更好的性能。

ED25519 SSH密钥是OpenSSH 6.5中引入的，因此任何现代操作系统都应包括创建它们的选项。 如果由于任何原因您与之交互的OS或GitLab实例不支持ED25519，则可以使用RSA。

注意: **Note:**
Omnibus不随OpenSSH一起提供，它使用您的GitLab服务器上的版本。 如果使用Omnibus，ED25519 SSH密钥，请确保已安装的OpenSSH版本是6.5版或更高版本。

### RSA SSH keys

RSA密钥是最常见的密钥，因此与可能具有旧OpenSSH版本的服务器最兼容。 如果GitLab服务器不适用于ED25519密钥，请使用RSA密钥。

最小密钥大小为1024位，默认为2048。如果希望生成更强的RSA密钥对，请使用比默认值高的位值指定`-b`标志。

## 生成新的SSH密钥对

在创建SSH密钥对之前，请确保了解[不同类型的密钥](#types-of-ssh-keys-and-which-to-choose).

要创建新的SSH密钥对：

1. 在Linux或macOS上打开终端，在Windows上打开Git Bash / WSL。
2. 生成新的ED25519 SSH密钥对:

   ```bash
   ssh-keygen -t ed25519 -C "email@example.com"
   ```

   或者，如果您想使用RSA:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "email@example.com"
   ```

   如果您有多个-C标志并想知道是哪个，则可使用`-C`标志在密钥中添加注释。 它是可选的。

3. 接下来，系统将提示您输入文件路径，以将SSH密钥对保存到此路径。如果您还没有SSH密钥对并且没有生成[deploy  key](#deploy-keys)，请通过按<kbd>Enter</kbd>使用建议的路径。使用建议的路径通常将使您的SSH客户端无需额外配置即可自动使用SSH密钥对。
  如果您已经拥有带有建议的文件路径的SSH密钥对，则需要输入新的文件路径，然后[声明主机](#working-with-non-default-ssh-key-pair-paths)，此SSH密钥 配对将在您的 `~/.ssh/config`文件中。

4. 确定路径后，将提示您输入密码以保护新的SSH密钥对。 最好使用密码，但这不是必需的，您可以通过按两次<kbd>Enter</kbd>来跳过创建密码的操作。
   无论如何，如果您想添加或更改SSH密钥对的密码，可以使用`-p`标志：

   ```
   ssh-keygen -p -f <keyname>
   ```

### OpenSSH < v7.8

在OpenSSH 7.8之前，SSH私钥的默认密码编码[不安全](https://latacora.micro.blog/the-default-openssh/)；it's only a single round of an MD5 hash.对于6.5到7.8的OpenSSH版本，您应该在`ssh-keygen`中使用`-o`选项[将您的私钥编码为新的、更安全的格式](https://superuser.com/questions/1455735/what-does-ssh-keygen-o-do#answer-1455738)。

If you already have an RSA SSH key pair to use with GitLab, consider upgrading it
to use the more secure password encryption format by using the following command
on the private key:

```bash
ssh-keygen -o -f ~/.ssh/id_rsa
```

Or generate a new RSA key:

```bash
ssh-keygen -o -t rsa -b 4096 -C "email@example.com"
```

Now, it's time to add the newly created public key to your GitLab account.

## Adding an SSH key to your GitLab account

1. Copy your **public** SSH key to the clipboard by using one of the commands below
   depending on your Operating System:

   **macOS:**

   ```bash
   pbcopy < ~/.ssh/id_ed25519.pub
   ```

   **WSL / GNU/Linux (requires the xclip package):**

   ```bash
   xclip -sel clip < ~/.ssh/id_ed25519.pub
   ```

   **Git Bash on Windows:**

   ```bash
   cat ~/.ssh/id_ed25519.pub | clip
   ```

   You can also open the key in a graphical editor and copy it from there,
   but be careful not to accidentally change anything.

   NOTE: **Note:**
   If you opted to create an RSA key, the name might differ.

1. Add your **public** SSH key to your GitLab account by:
   1. Clicking your avatar in the upper right corner and selecting **Settings**.
   1. Navigating to **SSH Keys** and pasting your **public** key from the clipboard into the **Key** field. If you:
      - Created the key with a comment, this will appear in the **Title** field.
      - Created the key without a comment, give your key an identifiable title like _Work Laptop_ or _Home Workstation_.
   1. Click the **Add key** button.

   NOTE: **Note:**
   If you manually copied your public SSH key make sure you copied the entire
   key starting with `ssh-ed25519` (or `ssh-rsa`) and ending with your email.

## Testing that everything is set up correctly

To test whether your SSH key was added correctly, run the following command in
your terminal (replacing `gitlab.com` with your GitLab's instance domain):

```bash
ssh -T git@gitlab.com
```

The first time you connect to GitLab via SSH, you will be asked to verify the
authenticity of the GitLab host you are connecting to.
For example, when connecting to GitLab.com, answer `yes` to add GitLab.com to
the list of trusted hosts:

```
The authenticity of host 'gitlab.com (35.231.145.151)' can't be established.
ECDSA key fingerprint is SHA256:HbW3g8zUjNSksFbqTiUWPWg2Bq1x8xdGUrliXFzSnUw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'gitlab.com' (ECDSA) to the list of known hosts.
```

NOTE: **Note:**
For GitLab.com, consult the
[SSH host keys fingerprints](../user/gitlab_com/index.md#ssh-host-keys-fingerprints),
to make sure you're connecting to the correct server.

Once added to the list of known hosts, you won't be asked to validate the
authenticity of GitLab's host again. Run the above command once more, and
you should only receive a _Welcome to GitLab, `@username`!_ message.

If the welcome message doesn't appear, run SSH's verbose mode by replacing `-T`
with `-vvvT` to understand where the error is.

## Working with non-default SSH key pair paths

If you used a non-default file path for your GitLab SSH key pair,
you must configure your SSH client to find your GitLab private SSH key
for connections to GitLab.

Open a terminal and use the following commands
(replacing `other_id_rsa` with your private SSH key):

```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/other_id_rsa
```

To retain these settings, you'll need to save them to a configuration file.
For OpenSSH clients this is configured in the `~/.ssh/config` file. In this
file you can set up configurations for multiple hosts, like GitLab.com, your
own GitLab instance, GitHub, Bitbucket, etc.

Below are two example host configurations using their own SSH key:

```conf
# GitLab.com
Host gitlab.com
  Preferredauthentications publickey
  IdentityFile ~/.ssh/gitlab_com_rsa

# Private GitLab instance
Host gitlab.company.com
  Preferredauthentications publickey
  IdentityFile ~/.ssh/example_com_rsa
```

Public SSH keys need to be unique to GitLab, as they will bind to your account.
Your SSH key is the only identifier you'll have when pushing code via SSH,
that's why it needs to uniquely map to a single user.

## Per-repository SSH keys

If you want to use different keys depending on the repository you are working
on, you can issue the following command while inside your repository:

```sh
git config core.sshCommand "ssh -o IdentitiesOnly=yes -i ~/.ssh/private-key-filename-for-this-repository -F /dev/null"
```

This will not use the SSH Agent and requires at least Git 2.10.

## Multiple accounts on a single GitLab instance

The [per-repository](#per-repository-ssh-keys) method also works for using
multiple accounts within a single GitLab instance.

Alternatively, it is possible to directly assign aliases to hosts in
`~.ssh/config`. SSH and, by extension, Git will fail to log in if there is
an `IdentityFile` set outside of a `Host` block in `.ssh/config`. This is
due to how SSH assembles `IdentityFile` entries and is not changed by
setting `IdentitiesOnly` to `yes`. `IdentityFile` entries should point to
the private key of an SSH key pair.

NOTE: **Note:**
Private and public keys should be readable by the user only. Accomplish this
on Linux and macOS by running: `chmod 0400 ~/.ssh/<example_ssh_key>` and
`chmod 0400 ~/.ssh/<example_sh_key.pub>`.

```conf
# User1 Account Identity
Host <user_1.gitlab.com>
  Hostname gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/<example_ssh_key1>

# User2 Account Identity
Host <user_2.gitlab.com>
  Hostname gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/<example_ssh_key2>
```

NOTE: **Note:**
The example `Host` aliases are defined as `user_1.gitlab.com` and
`user_2.gitlab.com` for efficiency and transparency. Advanced configurations
are more difficult to maintain; using this type of alias makes it easier to
understand when using other tools such as `git remote` subcommands. SSH
would understand any string as a `Host` alias thus `Tanuki1` and `Tanuki2`,
despite giving very little context as to where they point, would also work.

Cloning the `gitlab` repository normally looks like this:

```sh
git clone git@gitlab.com:gitlab-org/gitlab.git
```

To clone it for `user_1`, replace `gitlab.com` with the SSH alias `user_1.gitlab.com`:

```sh
git clone git@<user_1.gitlab.com>:gitlab-org/gitlab.git
```

Fix a previously cloned repository using the `git remote` command.

The example below assumes the remote repository is aliased as `origin`.

```sh
git remote set-url origin git@<user_1.gitlab.com>:gitlab-org/gitlab.git
```

## Deploy keys

### Per-repository deploy keys

Deploy keys allow read-only or read-write (if enabled) access to one or
multiple projects with a single SSH key pair.

This is really useful for cloning repositories to your Continuous
Integration (CI) server. By using deploy keys, you don't have to set up a
dummy user account.

If you are a project maintainer or owner, you can add a deploy key in the
project's **Settings > Repository** page by expanding the
**Deploy Keys** section. Specify a title for the new
deploy key and paste a public SSH key. After this, the machine that uses
the corresponding private SSH key has read-only or read-write (if enabled)
access to the project.

You can't add the same deploy key twice using the form.
If you want to add the same key to another project, please enable it in the
list that says 'Deploy keys from projects available to you'. All the deploy
keys of all the projects you have access to are available. This project
access can happen through being a direct member of the project, or through
a group.

Deploy keys can be shared between projects, you just need to add them to each
project.

### Global shared deploy keys

Global Shared Deploy keys allow read-only or read-write (if enabled) access to
be configured on any repository in the entire GitLab installation.

This is really useful for integrating repositories to secured, shared Continuous
Integration (CI) services or other shared services.
GitLab administrators can set up the Global Shared Deploy key in GitLab and
add the private key to any shared systems.  Individual repositories opt into
exposing their repository using these keys when a project maintainers (or higher)
authorizes a Global Shared Deploy key to be used with their project.

Global Shared Keys can provide greater security compared to Per-Project Deploy
Keys since an administrator of the target integrated system is the only one
who needs to know and configure the private key.

GitLab administrators set up Global Deploy keys in the Admin Area under the
section **Deploy Keys**. Ensure keys have a meaningful title as that will be
the primary way for project maintainers and owners to identify the correct Global
Deploy key to add. For instance, if the key gives access to a SaaS CI instance,
use the name of that service in the key name if that is all it is used for.
When creating Global Shared Deploy keys, give some thought to the granularity
of keys - they could be of very narrow usage such as just a specific service or
of broader usage for something like "Anywhere you need to give read access to
your repository".

Once a GitLab administrator adds the Global Deployment key, project maintainers
and owners can add it in project's **Settings > Repository** page by expanding the
**Deploy Keys** section and clicking **Enable** next to the appropriate key listed
under **Public deploy keys available to any project**.

NOTE: **Note:**
The heading **Public deploy keys available to any project** only appears
if there is at least one Global Deploy Key configured.

CAUTION: **Warning:**
Defining Global Deploy Keys does not expose any given repository via
the key until that repository adds the Global Deploy Key to their project.
In this way the Global Deploy Keys enable access by other systems, but do
not implicitly give any access just by setting them up.

## Applications

### Eclipse

If you are using [EGit](https://www.eclipse.org/egit/), you can [add your SSH key to Eclipse](https://wiki.eclipse.org/EGit/User_Guide#Eclipse_SSH_Configuration).

## SSH on the GitLab server

GitLab integrates with the system-installed SSH daemon, designating a user
(typically named `git`) through which all access requests are handled. Users
connecting to the GitLab server over SSH are identified by their SSH key instead
of their username.

SSH *client* operations performed on the GitLab server wil be executed as this
user. Although it is possible to modify the SSH configuration for this user to,
e.g., provide a private SSH key to authenticate these requests by, this practice
is **not supported** and is strongly discouraged as it presents significant
security risks.

The GitLab check process includes a check for this condition, and will direct you
to this section if your server is configured like this, e.g.:

```
$ gitlab-rake gitlab:check
# ...
Git user has default SSH configuration? ... no
  Try fixing it:
  mkdir ~/gitlab-check-backup-1504540051
  sudo mv /var/lib/git/.ssh/id_rsa ~/gitlab-check-backup-1504540051
  sudo mv /var/lib/git/.ssh/id_rsa.pub ~/gitlab-check-backup-1504540051
  For more information see:
  doc/ssh/README.md in section "SSH on the GitLab server"
  Please fix the error above and rerun the checks.
```

Remove the custom configuration as soon as you're able to. These customizations
are *explicitly not supported* and may stop working at any time.

## Troubleshooting

If on Git clone you are prompted for a password like `git@gitlab.com's password:`
something is wrong with your SSH setup.

- Ensure that you generated your SSH key pair correctly and added the public SSH
  key to your GitLab profile
- Try manually registering your private SSH key using `ssh-agent` as documented
  earlier in this document
- Try to debug the connection by running `ssh -Tv git@example.com`
  (replacing `example.com` with your GitLab domain)
