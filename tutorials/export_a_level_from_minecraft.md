# Export a Level from Minecraft

In this tutorial, you will learn how to export a level from Minecraft and import it into NovelCraft.

## Prerequisites

- **Lip command-line tool** You should install Lip in advance. For more information, refer to [Lip's documentation](https://lip.docs.litebds.com).

- **Minecraft Bedrock Dedicated Server(BDS)** You should download Minecraft Bedrock Dedicated Server in advance from [Minecraft.net](https://www.minecraft.net/en-us/download/server/bedrock/).

- **Minecraft Level** You should have a Minecraft level in advance. If you don't have one, when you run the BDS for the first time, it will generate a default level for you.

- **Minecraft client** You should have a Minecraft Bedrock Edition client in advance. You can download it from [Minecraft.net](https://www.minecraft.net/en-us/download). However, you may have to purchase a license to play it.

## Setting up BDS

Extract the BDS to a folder. For example, `D:\BDS`.

Open command prompt under the folder.

Run the following commands to install LiteLoaderBDS and MinecraftLevelExporter:

```shell
lip install github.com/tooth-hub/liteloaderbds github.com/tooth-hub/minecraftlevelexporter
```

If you cannot see bedrock_server_mod.exe, you may need to run LLPeEditor.exe to generate it.

Start the BDS by running bedrock_server_mod.exe.

## Use a custom level (optional)

You should close the BDS before you do this.

If you would like to use a custom level exported from Minecraft Bedrock Edition (with extension name .mcworld), you can unzip everything in the .mcworld file to a folder with the same name as the level name (called level folder) under the `worlds` folder under the BDS folder. For example, `D:\BDS\worlds\Bedrock level`.

Check if the content of levelname.txt file under the level folder, which is the level name, is the same as the level folder name. If not, change the level folder name to the same as the level name.

Start the BDS by running bedrock_server_mod.exe.

## Export a level from Minecraft

If you are running the BDS on your computer, you may need to disable the net isolation with the following command:

```shell
CheckNetIsolation.exe LoopbackExempt -a -p=S-1-15-2-1958404141-86561845-1752920682-3514627264-368642714-62675701-733520436
```

Connect to the BDS with your Minecraft client.

Wait till the world is loaded. If you would like to export a certain area, you should move your character to the area you want to export and wait till the world is loaded.

Run the following command **in the server console** to export the level:

```shell
export <start position> <end position>
```

For example:

```shell
export 0 0 0 100 100 100
```

Note that the end position is inclusive.

The exported level will be saved to the `plugins\level_exporter\exported.ncl` file under the BDS folder. For example, `D:\BDS\plugins\level_exporter\exported.ncl`.