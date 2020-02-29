---
title: shadermaterial创建雷达图扫描效果
date: 2019-09-10 20:56:01
tags:
---

公司项目要求在地图上生成一个雷达图扫描效果的功能，如下图：
![雷达效果图](/images/radar.jpg)

## 通过threejs创建圆形和扇形
```
        const radius = 100
        let group = new THREE.Group();
       
        //创建圆形区域并添加至场景
        let circleGeometry = new THREE.CircleBufferGeometry(radius, 72);
        circleGeometry.addAttribute('position', circleGeometry.attributes.position);
        let cMat = this.createCircleMaterial({
            color: color,
            radius: radius
        });
        this.circleMesh = new THREE.Mesh(circleGeometry, cMat);
        group.add(this.circleMesh );
        

        //创建扇形区域并添加至场景
        let sectorGeometry = new THREE.CircleBufferGeometry(radius, 72, 2 * Math.PI / 3, Math.PI / 3);
        sectorGeometry.addAttribute('position', sectorGeometry.attributes.position);
        let sMat = this.createSectorMaterial({
            color: color,
            radius: radius
        });
        this.sectorMesh = new THREE.Mesh(sectorGeometry, sMat);
        group.add(this.sectorMesh);
```
## 通过自定义定点着色器和片元着色器创建材质
```
    const createCircleMaterial = opt => {
        opt = opt || {};
        let ret = {
            uniforms: {
                color: { type: 'c', value: new THREE.Color(opt.color !== null ? opt.color : '#ff0000') },
                radius: { type: 'f', value: (typeof opt.radius === 'number' ? opt.radius : 3000.000) }
            },
            vertexShader: [
                'precision mediump float;',
                'precision mediump int;',
                'uniform mat4 modelViewMatrix;',
                'uniform mat4 projectionMatrix;',

                'attribute vec3 position;',
                'varying vec3 vPosition;',
                'void main()	{',
                //'vPosition = position.xyz + vec3(1000, 0,0);',
                'vPosition = position;',
                'gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );',

                '}'
            ].join('\n'),
            fragmentShader: [
                'precision mediump float;',
                'precision mediump int;',

                'uniform vec3 color;',
                'uniform float radius;',

                'varying vec3 vPosition;',

                'void main(){',
                'vec4 vcolor = vec4( color,1);',
                'float opacity=sqrt(vPosition.x*vPosition.x+vPosition.y*vPosition.y)/radius/1.5;',//透明度沿半径方向衰减
                'vcolor.a = opacity*opacity*opacity ;',
                'gl_FragColor = vcolor;',
                '}'
            ].join('\n'),
            transparent: true,
            side: THREE.DoubleSide
        };
        return new THREE.RawShaderMaterial(ret);

    };

    /**
     * 生成扇形材质
     * @param opt
     * @returns {RawShaderMaterial}
     */
    const = createSectorMaterial = opt => {
        opt = opt || {};
        let ret = {
            uniforms: {
                color: { type: 'c', value: new THREE.Color(opt.color !== null ? opt.color : '#ff0000') },
                radius: { type: 'f', value: (typeof opt.radius === 'number' ? parseFloat(opt.radius) : 3000.000) }
            },
            vertexShader: [
                'precision mediump float;',
                'precision mediump int;',
                'uniform mat4 modelViewMatrix;',
                'uniform mat4 projectionMatrix;',

                'attribute vec3 position;',
                'varying vec3 vPosition;',
                'void main()	{',
                'vPosition = position;',
                'gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );',
                '}'
            ].join('\n'),
            fragmentShader: [
                'precision mediump float;',
                'precision mediump int;',

                'uniform vec3 color;',
                'uniform float radius;',

                'varying vec3 vPosition;',

                'void main(){',
                'vec4 vcolor = vec4( color,1);',
                'float angel=atan(abs(vPosition.y)/(2.00000*radius-abs(vPosition.x)));',
                'float opacity= angel/(3.1415/4.00000);',//透明度随角度递增而增加
                'vcolor.a =opacity ;',
                'gl_FragColor = vcolor;',
                '}'
            ].join('\n'),
            transparent: true,
            side: THREE.DoubleSide
        };
        return new THREE.RawShaderMaterial(ret);
    };
```
## 循环转动扇形

```
        //循环转动扇形
        const run = () => {
            this.sectorMesh.rotateZ(-Math.PI / 60);
            this.animation = window.requestAnimationFrame(run);
        };
        run();
```
