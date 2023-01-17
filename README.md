## Simple Recoil System 
its not perfect but it worked for me so Yeah,

# HOW TO USE
*  Create a new Game object With The Same Position and Rotation as your Weapon 
*  Make Your Weapon A Child of the Created Game Object With Zeroed out postion and Rotation
*  Create a new C# script And Attach it to your weapon



# Code
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RecoilSystem : MonoBehaviour
{


    [Header("Recoil Transform")]
    [SerializeField] private weaponsSystem Weapon;

    [Space(10)]
    [Header("Animation Curve")]
    [SerializeField] private bool UseCurve = false;
    // All recoil curves must start and end with zero.
    [SerializeField] private AnimationCurve RotationX = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private AnimationCurve RotationY = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private AnimationCurve RotationZ = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private AnimationCurve PositionX = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private AnimationCurve PositionY = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private AnimationCurve PositionZ = AnimationCurve.EaseInOut(0.0f, 2.0f, 1.0f, 0.0f);
    [SerializeField] private float AnimationcurveTime = 0;
    [SerializeField] private float duration = 1.0f;
    private Vector3 InitPosition = Vector3.zero;
    private Quaternion InitRotation = Quaternion.identity;
    private float TimePassed;
    private Coroutine Recoil;
    private IEnumerator StartRecoil()
    {

            TimePassed = 0;
            AnimationcurveTime = 0;
            while (TimePassed < duration)
            {
                TimePassed += Time.deltaTime;
                AnimationcurveTime = TimePassed / duration;
                transform.localRotation = Quaternion.Lerp(transform.localRotation, Quaternion.Euler(CalculateNextRotation()) , AnimationcurveTime);
                transform.localPosition = Vector3.Lerp(transform.localPosition, CalculateNextPosition(), AnimationcurveTime);
                yield return null;
            }
    }

    private Vector3 CurrentRotation, CurrentPosition;
    public Vector3 CalculateNextRotation()
    {
        float Rotationx = RotationX.Evaluate(AnimationcurveTime);
        float Rotationy = RotationY.Evaluate(AnimationcurveTime); 
        float Rotationz = RotationZ.Evaluate(AnimationcurveTime);

        if (Weapon != null)
        {
        if (Weapon.IsAiming == true)
        {
                CurrentRotation =
                 new Vector3(
               Rotationx,
               Rotationy,
               Rotationz
                ) / 2;
        }
        else
        {
            CurrentRotation = 
                new Vector3(
                Rotationx,
                Rotationy,
                Rotationz

               );
        }
        }
        else
        {
            CurrentRotation =
                new Vector3(
                Rotationx,
                Rotationy,
                Rotationz

               );
        }

        return CurrentRotation;
    }
    public Vector3 CalculateNextPosition()
    {
        float Positionx = PositionX.Evaluate(AnimationcurveTime);
        float Positiony = PositionY.Evaluate(AnimationcurveTime);
        float Positionz = PositionZ.Evaluate(AnimationcurveTime);

        if (Weapon != null)
        {
            if (Weapon.IsAiming == true)
            {
                CurrentPosition =
                    new Vector3
                    (
                    Positionx,
                    Positiony,
                    Positionz
                    ) / 2;
            }
            else
            {
                CurrentPosition =
                    new Vector3
                    (
                    Positionx,
                    Positiony,
                    Positionz
                    );
            }
        }
        else
        {
            CurrentPosition =
                new Vector3
                (
                Positionx,
                Positiony,
                Positionz
                );
        }
        return CurrentPosition;
    }
    public void ApplyRecoil()
    {
        InitPosition = transform.localPosition;
        InitRotation = transform.localRotation;
        if (Recoil != null)
        {
            StopCoroutine(Recoil);
        }
        Recoil = StartCoroutine(StartRecoil());

    }

    
}


```
* now on your weapon Script when you shot just call this Function: **ApplyRecoil()**
* And last make A bool with the name **IsAiming**

# Done 



## 📞 Issues
If you have any issues feel free to open an issue or contact me on Discord (SHETER#6133).
