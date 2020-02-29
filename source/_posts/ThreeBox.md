---
title: three.js和mapbox的融合
date: 2018-03-19 20:20:12
tags:
---

## 经纬度转墨卡托投影公式
（地理坐标系的经纬度转投影坐标系的米，推导公式可查阅相关资料）

```
    const WORLD_SIZE = 512;
    const MERCATOR_A = 6378137.0;

    //墨卡托投影相关系数
    const ThreeboxConstants =  {
        WORLD_SIZE: WORLD_SIZE,
        PROJECTION_WORLD_SIZE: WORLD_SIZE / (MERCATOR_A * Math.PI) / 2,
        MERCATOR_A: MERCATOR_A,
        DEG2RAD: Math.PI / 180,
        RAD2DEG: 180 / Math.PI,
        EARTH_CIRCUMFERENCE: 40075000 // 米
    };

    //世界坐标系和投影坐标系的比例转换（每个像素代表的墨卡托投影系下的长度值）
    const projectedUnitsPerMeter = function (latitude) {
         return Math.abs(ThreeboxConstants.WORLD_SIZE * (1 / Math.cos(latitude * Math.PI / 180)) / ThreeboxConstants.EARTH_CIRCUMFERENCE);
    }
    var projected = [
        -ThreeboxConstants.MERCATOR_A * coords[0] * ThreeboxConstants.DEG2RAD * ThreeboxConstants.PROJECTION_WORLD_SIZE,
        -ThreeboxConstants.MERCATOR_A * Math.log(Math.tan((Math.PI * 0.25) + (0.5 * coords[1] * ThreeboxConstants.DEG2RAD))) * ThreeboxConstants.PROJECTION_WORLD_SIZE
    ];

    var pixelsPerMeter = projectedUnitsPerMeter(coords[1]);

    //z
    var height = coords[2] || 0;
    projected.push(height * pixelsPerMeter);

    var result = new THREE.Vector3(projected[0], projected[1], projected[2]);

    return result;
```