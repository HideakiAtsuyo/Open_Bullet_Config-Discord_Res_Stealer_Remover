# Open Bullet Config - Discord Res Stealer Remover

A little example to clean res stealer in Open Bullet configs using Discord Webhooks :)

[Download Built Release](https://github.com/HideakiAtsuyo/Open_Bullet_Config-Discord_Res_Stealer_Remover/releases/download/1.0/Debug.rar)

Made for [Ethvn Twitter](https://twitter.com/3thvn_) / Discord: Ethvn#1805

```cs
using Leaf.xNet;
using Newtonsoft.Json.Linq;
using System;
using System.IO;
using System.Text.RegularExpressions;

namespace OpenBulletConfig_ResStealerRemover
{
    internal class Program
    {
        private static string configPath = string.Empty, configFileContent = string.Empty;
        static void Main(string[] args)
        {
            if(args.Length != 0)
                configPath = args[0].Replace("\"", string.Empty);

            while (!File.Exists(configPath))
            {
                Console.WriteLine("Config File Path: ");
                configPath = Console.ReadLine().Replace("\"", string.Empty);
            }
            configFileContent = File.ReadAllText(configPath);
            Match match = Regex.Match(configFileContent, @"(?:http(s?)\:\/\/)(?:canary.|ptb.)?(?:discord|discordapp).com\/api\/webhooks\/\d*\/.*");
            if (match.Success)
            {
                using (HttpRequest req = new HttpRequest())
                {
                    req.IgnoreProtocolErrors = true;
                    try
                    {
                        string webhook = match.Value.Replace("\"", string.Empty).Split(' ')[0];
                        string res = req.Get(webhook).ToString();
                        JObject json = JObject.Parse(res);

                        if (!json.ContainsKey("message"))
                        {
                            Console.WriteLine("Discord Res Stealer Removed !");
                            req.Delete(webhook);
                        }
                        else
                        {
                            Console.WriteLine("Clean !");
                        }
                    } catch (Exception ex)
                    {
                        Console.WriteLine("Clean !");
                    }
                }
            }
            Console.ReadLine();
        }
    }
}
```
