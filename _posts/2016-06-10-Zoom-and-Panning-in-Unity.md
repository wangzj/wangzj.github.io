---
layout: post
title: "如何实现缩放和平移"
date: 15:03:54
categories: unity
imageuri: pinch_to_zoom.jpg
---

假设在场景里放入了一张大地图，地图的尺寸超过了设备屏幕的大小。通过平移(panning)
和缩放(zoom)地图，可以浏览地图里的各个局部。


### 用鼠标平移和缩放

和普通的拖拽不同，平移不会改变地图上各个对象的坐标位置。只要控制镜头的位置，可以
在屏幕上显示观察的区域。下面我们用脚本实现[[1][ZOOM]]。

{% highlight c# %}

Vector3 currPos;
Vector3 lastPos;

void Update () {
    currPos = Camera.main.ScreenToWorldPoint (Input.mousePosition);
    currPos.z = 0;

    UpdateCameraLocation ();

    lastPos = currPos;
}

void UpdateCameraLocation () {
    if (EventSystem.current.IsPointerOverGameObject ())
        return;

    // Use right or middle key of mouse
    if (Input.GetMouseButton (1) || Input.GetMouseButton (2)) {
        Camera.main.transform.Translate (lastPos - currPos);
    }
}

{% endhighlight %}

这样可以使用鼠标的右键和滚轮拖拽地图了。为了更清楚地查看地图的细节，还需要允许
放大和缩小地图。这里的摄像机使用的是正交的摄像机(orthographic)，修改相机携带的
orthographicSize属性，可以改变视野的大小。

{% highlight c# %}

public float minScale = 0.6f;
public float maxScale = 6.0f;

void UpdateCameraLocation () {

    ...

    /** Append at the bottom **/
    // Zoom by scroll wheel
    Camera.main.orthographicSize -= Camera.main.orthographicSize * Input.GetAxis ("Mouse ScrollWheel");
    Camera.main.orthographicSize = Mathf.Clamp (Camera.main.orthographicSize, minScale, maxScale);
}

{% endhighlight %}

注意到，镜头的视角溢出地图的区域可能不是期望的事，镜头中心的位置需要进一步限制。

{% highlight c# %}

public Vector3 worldLowerLeft;
public Vector3 worldUpperRight;

void EnsureCameraInFOV () {
    Vector3 minCorner = Camera.main.ViewportToWorldPoint (Vector3.zero);
    Vector3 maxCorner = Camera.main.ViewportToWorldPoint (Vector3.one);
    Vector3 boundingBoxSize = maxCorner - minCorner;
    float limXLow = worldLowerLeft.x + boundingBoxSize.x / 2;
    float limXHigh = worldUpperRight.x - boundingBoxSize.x / 2;
    float limYLow = worldLowerLeft.y + boundingBoxSize.y / 2;
    float limYHigh = worldUpperRight.y - boundingBoxSize.y / 2;
    float x = worldUpperRight.x / 2 + worldLowerLeft.x / 2;
    float y = worldUpperRight.y / 2 + worldLowerLeft.y / 2;

    if (limYLow < limYHigh && limXLow < limXHigh) {
        x = Mathf.Clamp (Camera.main.transform.position.x, limXLow, limXHigh);
        y = Mathf.Clamp (Camera.main.transform.position.y, limYLow, limYHigh);
    }

    Camera.main.transform.position = new Vector3 (x, y, Camera.main.transform.position.z);
}

void Update () {

    ...
    UpdateCameraLocation ();

    /* Add the following line */
    EnsureCameraInFOV (); // make sure camera in predefined field of view

    ...
}

{% endhighlight %}


### 拉捏缩放(Pinch-to-zoom)

在移动设备上，通过捏合和拉开手指的触摸手势，实现缩小和放大。[[2][PINCH-ZOOM]]


### 材料

[[1][ZOOM]] http://www.completeunitydeveloper.com/blog/how-to-pan-and-zoom-a-map-in-unity <br>
[[2][PINCH-ZOOM]] http://www.theappguruz.com/blog/pinch-zoom-panning-unity

[ZOOM]: http://www.completeunitydeveloper.com/blog/how-to-pan-and-zoom-a-map-in-unity
[PINCH-ZOOM]: http://www.theappguruz.com/blog/pinch-zoom-panning-unity
