---
description: TwinCAT 3 XAE - a simple program to test motor
---

# Axis Control with EtherCAT

* This is TwinCAT V3.1.4024.66

### Create TwinCAT Project&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 115630.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 115709 (2).png" alt=""><figcaption></figcaption></figure>

* I/O -> Devices -> Scan Devices&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 115828 (1).png" alt=""><figcaption></figcaption></figure>

* Your PLC Devices should pop up after scanning

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 115938 (1).png" alt=""><figcaption></figcaption></figure>

### Setting Encoder Parameters&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 120031 (1).png" alt=""><figcaption></figcaption></figure>

* Based on the Encodfer parameter received by the PLC machine, set up encoders. For this setup, I am using 800nm resolution Absolute encoder linear stage with 85mm travel distance.&#x20;
  * 0.0008 mm/Inc
  * Modulo Factor - 85 (this variable can be skipped)
  * Absolute Reference system &#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 120540 (2).png" alt=""><figcaption></figcaption></figure>

### Setting PLC Program

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 120636 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 120710 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 120757 (1).png" alt=""><figcaption></figcaption></figure>

In Reference, there should be a default of TC2\_Standard, TC2\_System, and TC3\_Module. However, we need to add Library "TC2\_MC2"

* Reference -->   Add Library&#x20;
* &#x20;Add Motion/PTP/TC2\_MC2

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 121021 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 121422 (1).png" alt=""><figcaption></figcaption></figure>

Now, the TC2\_MC2 library should be added to the Project.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 121301 (1).png" alt=""><figcaption></figcaption></figure>

### Axis moving Code&#x20;

```
PROGRAM MAIN
VAR
    Axis1_Activate : BOOL := TRUE;
    Axis2_Activate : BOOL := TRUE;
    Axis3_Activate : BOOL := TRUE;
    Axis4_Activate : BOOL := TRUE;

    Axis1_PosForward  : LREAL := 80.0;
    Axis1_PosBackward : LREAL := 5.0;
    Axis1_Velocity    : LREAL := 100.0;
    Axis1_Accel       : LREAL := 100.0;
    Axis1_Decel       : LREAL := 100.0;

    Axis2_PosForward  : LREAL := 3.0;
    Axis2_PosBackward : LREAL := 81.0;
    Axis2_Velocity    : LREAL := 70.0;
    Axis2_Accel       : LREAL := 1500.0;
    Axis2_Decel       : LREAL := 1500.0;

    Axis3_PosForward  : LREAL := 80.0;
    Axis3_PosBackward : LREAL := 5.0;
    Axis3_Velocity    : LREAL := 70.0;
    Axis3_Accel       : LREAL := 150.0;
    Axis3_Decel       : LREAL := 150.0;

    Axis4_PosForward  : LREAL := 80.0;
    Axis4_PosBackward : LREAL := 5.0;
    Axis4_Velocity    : LREAL := 70.0;
    Axis4_Accel       : LREAL := 150.0;
    Axis4_Decel       : LREAL := 150.0;

    CycleDelay : TIME := T#3S;

    fbBreakTimer : TON;
    bStartBreak  : BOOL := FALSE;

    nStep : INT := 0;

    bDirectionForwardAxis1 : BOOL := TRUE;
    bDirectionForwardAxis2 : BOOL := TRUE;
    bDirectionForwardAxis3 : BOOL := TRUE;
    bDirectionForwardAxis4 : BOOL := TRUE;

    mcPowerAxis1        : MC_Power;
    mcMoveAbsoluteAxis1 : MC_MoveAbsolute;
    Axis1Ref            : AXIS_REF;

    mcPowerAxis2        : MC_Power;
    mcMoveAbsoluteAxis2 : MC_MoveAbsolute;
    Axis2Ref            : AXIS_REF;

    mcPowerAxis3        : MC_Power;
    mcMoveAbsoluteAxis3 : MC_MoveAbsolute;
    Axis3Ref            : AXIS_REF;

    mcPowerAxis4        : MC_Power;
    mcMoveAbsoluteAxis4 : MC_MoveAbsolute;
    Axis4Ref            : AXIS_REF;
END_VAR

```

```
CASE nStep OF

    0:
        IF Axis1_Activate THEN
            mcPowerAxis1.Enable := TRUE;
            mcPowerAxis1.Enable_Positive := TRUE;
            mcPowerAxis1.Enable_Negative := TRUE;
        END_IF;

        IF Axis2_Activate THEN
            mcPowerAxis2.Enable := TRUE;
            mcPowerAxis2.Enable_Positive := TRUE;
            mcPowerAxis2.Enable_Negative := TRUE;
        END_IF;

        IF Axis3_Activate THEN
            mcPowerAxis3.Enable := TRUE;
            mcPowerAxis3.Enable_Positive := TRUE;
            mcPowerAxis3.Enable_Negative := TRUE;
        END_IF;

        IF Axis4_Activate THEN
            mcPowerAxis4.Enable := TRUE;
            mcPowerAxis4.Enable_Positive := TRUE;
            mcPowerAxis4.Enable_Negative := TRUE;
        END_IF;

        nStep := 1;


    1:
        IF (NOT Axis1_Activate OR mcPowerAxis1.Status) AND
           (NOT Axis2_Activate OR mcPowerAxis2.Status) AND
           (NOT Axis3_Activate OR mcPowerAxis3.Status) AND
           (NOT Axis4_Activate OR mcPowerAxis4.Status) THEN
            nStep := 2;
        END_IF;


    2:
        IF Axis1_Activate THEN
            mcMoveAbsoluteAxis1.Position     := SEL(bDirectionForwardAxis1, Axis1_PosBackward, Axis1_PosForward);
            mcMoveAbsoluteAxis1.Velocity     := Axis1_Velocity;
            mcMoveAbsoluteAxis1.Acceleration := Axis1_Accel;
            mcMoveAbsoluteAxis1.Deceleration := Axis1_Decel;
            mcMoveAbsoluteAxis1.Execute      := TRUE;
        END_IF;

        IF Axis2_Activate THEN
            mcMoveAbsoluteAxis2.Position     := SEL(bDirectionForwardAxis2, Axis2_PosBackward, Axis2_PosForward);
            mcMoveAbsoluteAxis2.Velocity     := Axis2_Velocity;
            mcMoveAbsoluteAxis2.Acceleration := Axis2_Accel;
            mcMoveAbsoluteAxis2.Deceleration := Axis2_Decel;
            mcMoveAbsoluteAxis2.Execute      := TRUE;
        END_IF;

        IF Axis3_Activate THEN
            mcMoveAbsoluteAxis3.Position     := SEL(bDirectionForwardAxis3, Axis3_PosBackward, Axis3_PosForward);
            mcMoveAbsoluteAxis3.Velocity     := Axis3_Velocity;
            mcMoveAbsoluteAxis3.Acceleration := Axis3_Accel;
            mcMoveAbsoluteAxis3.Deceleration := Axis3_Decel;
            mcMoveAbsoluteAxis3.Execute      := TRUE;
        END_IF;

        IF Axis4_Activate THEN
            mcMoveAbsoluteAxis4.Position     := SEL(bDirectionForwardAxis4, Axis4_PosBackward, Axis4_PosForward);
            mcMoveAbsoluteAxis4.Velocity     := Axis4_Velocity;
            mcMoveAbsoluteAxis4.Acceleration := Axis4_Accel;
            mcMoveAbsoluteAxis4.Deceleration := Axis4_Decel;
            mcMoveAbsoluteAxis4.Execute      := TRUE;
        END_IF;

        nStep := 3;


    3:
        IF (NOT Axis1_Activate OR mcMoveAbsoluteAxis1.Done) AND
           (NOT Axis2_Activate OR mcMoveAbsoluteAxis2.Done) AND
           (NOT Axis3_Activate OR mcMoveAbsoluteAxis3.Done) AND
           (NOT Axis4_Activate OR mcMoveAbsoluteAxis4.Done) THEN

            IF Axis1_Activate THEN mcMoveAbsoluteAxis1.Execute := FALSE; END_IF;
            IF Axis2_Activate THEN mcMoveAbsoluteAxis2.Execute := FALSE; END_IF;
            IF Axis3_Activate THEN mcMoveAbsoluteAxis3.Execute := FALSE; END_IF;
            IF Axis4_Activate THEN mcMoveAbsoluteAxis4.Execute := FALSE; END_IF;

            IF Axis1_Activate THEN bDirectionForwardAxis1 := NOT bDirectionForwardAxis1; END_IF;
            IF Axis2_Activate THEN bDirectionForwardAxis2 := NOT bDirectionForwardAxis2; END_IF;
            IF Axis3_Activate THEN bDirectionForwardAxis3 := NOT bDirectionForwardAxis3; END_IF;
            IF Axis4_Activate THEN bDirectionForwardAxis4 := NOT bDirectionForwardAxis4; END_IF;

            nStep := 4;
        END_IF;


    4:
        bStartBreak := TRUE;
        fbBreakTimer(IN := bStartBreak, PT := CycleDelay);

        IF fbBreakTimer.Q THEN
            bStartBreak := FALSE;
            fbBreakTimer(IN := FALSE);
            nStep := 2;
        END_IF;


    100:
        mcPowerAxis1.Enable := FALSE;
        mcPowerAxis2.Enable := FALSE;
        mcPowerAxis3.Enable := FALSE;
        mcPowerAxis4.Enable := FALSE;

        mcMoveAbsoluteAxis1.Execute := FALSE;
        mcMoveAbsoluteAxis2.Execute := FALSE;
        mcMoveAbsoluteAxis3.Execute := FALSE;
        mcMoveAbsoluteAxis4.Execute := FALSE;

END_CASE;


mcPowerAxis1(
    Axis := Axis1Ref,
    Enable := ,
    Enable_Positive := ,
    Enable_Negative := ,
    Override := ,
    BufferMode := ,
    Options := ,
    Status => ,
    Busy => ,
    Error => ,
    ErrorID => 
);

mcPowerAxis2(
    Axis := Axis2Ref,
    Enable := ,
    Enable_Positive := ,
    Enable_Negative := ,
    Override := ,
    BufferMode := ,
    Options := ,
    Status => ,
    Busy => ,
    Error => ,
    ErrorID => 
);

mcPowerAxis3(
    Axis := Axis3Ref,
    Enable := ,
    Enable_Positive := ,
    Enable_Negative := ,
    Override := ,
    BufferMode := ,
    Options := ,
    Status => ,
    Busy => ,
    Error => ,
    ErrorID => 
);

mcPowerAxis4(
    Axis := Axis4Ref,
    Enable := ,
    Enable_Positive := ,
    Enable_Negative := ,
    Override := ,
    BufferMode := ,
    Options := ,
    Status => ,
    Busy => ,
    Error => ,
    ErrorID => 
);


mcMoveAbsoluteAxis1(
    Axis := Axis1Ref,
    Execute := ,
    Position := ,
    Velocity := ,
    Acceleration := ,
    Deceleration := ,
    Jerk := ,
    BufferMode := ,
    Options := ,
    Done => ,
    Busy => ,
    Active => ,
    CommandAborted => ,
    Error => ,
    ErrorID => 
);

mcMoveAbsoluteAxis2(
    Axis := Axis2Ref,
    Execute := ,
    Position := ,
    Velocity := ,
    Acceleration := ,
    Deceleration := ,
    Jerk := ,
    BufferMode := ,
    Options := ,
    Done => ,
    Busy => ,
    Active => ,
    CommandAborted => ,
    Error => ,
    ErrorID => 
);

mcMoveAbsoluteAxis3(
    Axis := Axis3Ref,
    Execute := ,
    Position := ,
    Velocity := ,
    Acceleration := ,
    Deceleration := ,
    Jerk := ,
    BufferMode := ,
    Options := ,
    Done => ,
    Busy => ,
    Active => ,
    CommandAborted => ,
    Error => ,
    ErrorID => 
);

mcMoveAbsoluteAxis4(
    Axis := Axis4Ref,
    Execute := ,
    Position := ,
    Velocity := ,
    Acceleration := ,
    Deceleration := ,
    Jerk := ,
    BufferMode := ,
    Options := ,
    Done => ,
    Busy => ,
    Active => ,
    CommandAborted => ,
    Error => ,
    ErrorID => 
);

```

Above code provides the bare minimum logic required to move each axis between two positions + timer break&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 122138 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 122201 (2).png" alt=""><figcaption></figcaption></figure>

Now, Each Axis should be linked to the object that was instantiated in the code.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 122308 (1).png" alt=""><figcaption></figcaption></figure>

### Running Project&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 122543 (1).png" alt=""><figcaption></figcaption></figure>

* Check \[Autostart PLC Boot Project] when running the project.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-04-10 123029 (2).png" alt=""><figcaption></figcaption></figure>

If you go to the \[Login], you should be able to see all the active variables

