# Launching Visual Studio Code

[github.com](https://github.com/Clouder0/SiyuanYuque)Clouder0

## SiyuanYuque

Sync SiYuanNote & Yuque.

*   Sync Setting in doc attribute.
*   Batch Sync Via SQL
*   SiYuan Image Hosting Supported
*   Internal Link Supported

## Install

Use pip to install.

pip install SiyuanYuque

Execute like this:

python -m SiyuanYuque

Remember to create a `sqconfig.toml` config file in the current directory.

user\_token = "" siyuan\_token = "" api\_host = "https://www.yuque.com/api/v2" last\_sync\_time = "20210915225457"

Fill in your Yuque user\_token and siyuan\_token.

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F41664195%2F133458286-41abaf7a-aab2-4c98-a758-e29f7512a8f6.png)](https://user-images.githubusercontent.com/41664195/133458286-41abaf7a-aab2-4c98-a758-e29f7512a8f6.png)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F41664195%2F133458339-69a698d8-a133-4ef8-9419-ccec7354ddc7.png)](https://user-images.githubusercontent.com/41664195/133458339-69a698d8-a133-4ef8-9419-ccec7354ddc7.png)

## Set Atrribute in SiyuanNote

You can only sync documents to Yuque.

Set Attributes like this:

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F41664195%2F133459061-737ca0ec-aa47-4294-b5db-4b6bb8d6a02d.png)](https://user-images.githubusercontent.com/41664195/133459061-737ca0ec-aa47-4294-b5db-4b6bb8d6a02d.png)

yuque: true yuque-workspace: your workspace

Workspace format: `username/repo`

Then run `python -m SiyuanYuque`, and check the attributes again.

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F41664195%2F133459218-8bc181aa-2429-4075-b8b3-2b9af4f6ca7f.png)](https://user-images.githubusercontent.com/41664195/133459218-8bc181aa-2429-4075-b8b3-2b9af4f6ca7f.png)

You'll see `yuque-id` appended to your document's attributes. **Don't manually modify this unless you know what you are doing.**

That's the basic usage for the time being.

**Remember not to edit the documents sync from SiYuan, as the update will be lost upon the next sync.**

## Custom Sync

It is supported to sync documents by SQL.

A simple example:

user\_token = "" siyuan\_token = "" api\_host = "https://www.yuque.com/api/v2" last\_sync\_time = "20210916223903" assets\_replacement = "https://b3logfile.com/siyuan/1609132319768/assets" \[\[custom\_sync\]\] sql = "select \* from blocks where hpath like '%Math/%' and type='d'" yuque-workspace = "clouder0/gaokao"

Multiple custom syncs can be defined.

user\_token = "" siyuan\_token = "" api\_host = "https://www.yuque.com/api/v2" last\_sync\_time = "20210916223903" assets\_replacement = "https://b3logfile.com/siyuan/1609132319768/assets" \[\[custom\_sync\]\] sql = "select \* from blocks where hpath like '%Math/%' and type='d'" yuque-workspace = "clouder0/gaokao" \[\[custom\_sync\]\] sql = "select \* from blocks where hpath like '%Chinese/%' and type='d'" yuque-workspace = "clouder0/gaokao"

## More Config

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F41664195%2F133639009-77031416-b9cd-4470-aa90-3f3ba00fbbd4.png)](https://user-images.githubusercontent.com/41664195/133639009-77031416-b9cd-4470-aa90-3f3ba00fbbd4.png)

yuque-public: 1 for public and 0 for private.

yuque-slug: the slug of the document. `https://www.yuque.com/siyuannote/docs/siyuanyuque`

## Assets Replacement

Replace the `assets` string in your markdown content to support SiYuan online image.

## Internal Link

SiYuan-Setting: Ref Block: Anchor Text with block URL.

This script will replace `siyuan://blocks` with `https://yuque.com/{workspace}` so that your ref blocks that have been exported and in the same workspace of yuque will be accessible.

[查看原网页: github.com](https://github.com/Clouder0/SiyuanYuque)