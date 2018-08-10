---
layout: post
title: Juice in Unity \#1 - Screenshake
date: 2018-08-08 19:31:20 -0300
description: Simple guide and thoughts on Screenshaking!
img: theshake.jpg
tags: [Gameplay Programming, Unity, Game Development, Screenshake]
---

First things first, if you don't know what is the **JUICE**, I feel sorry for you. There is a moment, in every game developer life, where they get to meet the **JUICE**. It's is, for sure, a life changing moment. That being said, if you don't know what is the **JUICE** you probably have already thought: "My game is bad and I don't know why" or "My game has a fun mechanic but is bad, why?" - The answer to all your thoughts and problems probably are: **THE JUICE**!. 

Watch [Juice it or Lose It](https://www.youtube.com/watch?v=Fy0aCDmgnxg) and [The Art of Screenshake](https://www.youtube.com/watch?v=AJdEqssNZ-U) and watch your games improve in 100% by just watching a couple lectures!

I'm going to write here about a simple way to add Screen Shake to your game in Unity, it is so simple that it will take just a few hundred words and a simple script.

## Setting the Project

What do we need to create a simple project to implement a screen shake? Just a camera and an image in the background. Yes, you will need the image otherwise you won't see the camera shaking, I tried that.

Ps: The code for this project is open source so you can view, download and test it. [You can find it here](https://github.com/guilhermepo2/unity/tree/master/Screenshake).

## The Code

Some words before, I like using this script on the **Main Camera** itself, you would need a reference for it but that's easy to set up, right? And as you are going to mess with the camera position, it is easier to do it inside the camera.

**How it is done?**

As everything in computer programming, there are many ways to solve a problem. The approach I will be using here is simple: have a shake amount and create a position offset based on that amount, the offset will be added to the camera each frame, then the offset will change based, again, on the shake amount, this will happen during a decay time.

**Variables**

```
public float screenShakeDecay = .5f; // Decay Time
private Vector3 m_screenShake;       // Offset to be added on the camera
private float m_screenShakeAmount;   // Amount of Offset to be added on the camera
```

The variable `screenShakeDecay` is public so we can easily tune it in the inspector, a default value of 0.5f seems good to me and I usually don't change it.

Once we have a `screenShakeAmount`, how do we shake things up? (ha!)

**The Update Method**

```
void Update () {
    if(m_screenShakeAmount > 0) {
        float t_x = Random.Range(-m_screenShakeAmount, m_screenShakeAmount);
        float t_y = Random.Range(-m_screenShakeAmount, m_screenShakeAmount);
        
        m_screenShake = new Vector3(t_x,
                                    t_y,
                                    0f);

        m_screenShakeAmount -= Time.deltaTime * screenShakeDecay;
    } else {
        m_screenShake = Vector3.zero;
        transform.position = new Vector3(0f, 0f, -10f);
    }

    transform.position += m_screenShake;

}
```

We have a couple things happening here, let's go through steps.

The variables `t_x` and `t_y` are being created just to easily try different values for the screen shake, you are probably better *not* creating them. Next we have the `m_screenShake` creation, using the previous two variables and `0f` on the z axis, in 2D games you won't change de z axis. Never.

The `screenShake` is on a random range between the negative and positive values for the amount, this is the most simple way of doing it. After that, we *decrement* the amount by deltaTime and our decay variable. This is important because this code is using this variable to stop shaking **and** to progressively shake less as time goes by.

On the `else` part of the script, `m_screenShakeAmount` reached zero and the screen shake should end, so we explicitly set `m_screenShake` to zero. **I'm resetting the camera position but this is for testing purposes only, in a real game you would want it to smoothly return to the origin, (0, 0, -10) in my case, or for to smoothly follow the player.**

At the end of the `Update()` method, we are adding the screen shake to our camera position. Simple, right?

**But how do I effectively shake it?**

Now it's for our last and most important method, the `ShakeScreen` method!

```
public void ShakeScreen(float toShake) {
	m_screenShakeAmount = toShake;
}
```

Yep. That's as simple as one line of code. Adding a value to `m_screenShakeAmount` is enough to make it as we are handling it on `Update()`. It is important this method is **public** because we want to call it from different points in our game and it is important for it to have a **toShake** parameter because different objects in our games will shake the camera in different quantities.

But, how **much** screem shake amount?

This question is one that you, and only you, can answer. Test a lot of diferent values, start with 1, goes to 2, 0.5, 10, 500, whatever value you want...

**Give me the full script!**

Now you can be happy with this complex 30 line Screenshake script!

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Screenshake : MonoBehaviour {

	public float screenShakeDecay = .5f;
	private Vector3 m_screenShake;
	private float m_screenShakeAmount;

	void Update () {
		if(m_screenShakeAmount > 0) {
			float t_x = Random.Range(-m_screenShakeAmount, m_screenShakeAmount);
			float t_y = Random.Range(-m_screenShakeAmount, m_screenShakeAmount);
			m_screenShake = new Vector3(t_x,
										t_y,
										0f);
			m_screenShakeAmount -= Time.deltaTime * screenShakeDecay;
		} else {
			m_screenShake = Vector3.zero;
			transform.position = new Vector3(0f, 0f, -10f);
		}

		transform.position += m_screenShake;
	}

	public void ShakeScreen(float toShake) {
		m_screenShakeAmount = toShake;
	}
}
```

Just a quick reminder that you can find this code on [GitHub](https://github.com/guilhermepo2/unity/tree/master/Screenshake).

Have a good day and always remember the **JUICE!**