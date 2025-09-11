---
title: "C++并发编程"
date: 2022-08-11T10:43:52+08:00
tags: ["c++","并发"]
categories: [""]
draft: true
---
client_id:5a8018e2-7543-4ecc-b9b3-1871c77c8707
client_secret:s.m~5bHAoxVqb_aZJe0kJ.CZ3WUVRt_f5.

cbe5565d-2458-461e-b860-2790a566371e

d3_hPHkD_sLIh.YvQ_-3tcTS_0wmld~9w1



mkdir -p /data/workspace/hozon-senseauto  
cd /data/workspace/hozon-senseauto  
   
# 下载项目分支 （开发者一般下载hozon_dev分支）  
repo init -u ssh://gerrit.senseauto.com/senseauto_manifest \  
          -b develop -m hozon_assd_all_dev.xml \  
          --repo-url=ssh://gerrit.senseauto.com:29418/senseauto_repo --repo-branch=develop --no-repo-verify  
    
repo sync -j4  
repo forall -j 4 -p -c 'git lfs pull'  
repo start hozon_dev --all