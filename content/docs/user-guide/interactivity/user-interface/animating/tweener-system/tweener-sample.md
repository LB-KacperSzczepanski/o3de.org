---
description: ' Open the Scripted Entity Tweener sample level in Open 3D Engine to see tweeners
  in action. '
title: Tweener Sample Level
---

{{< preview-migrated >}}

You can use the [Samples Project](/docs/userguide/samples/projects/samples.md) to see an example level that uses tweeners\.

**To see example Scripted Entity Tweeners in action**

1. In O3DE Editor, in the [Samples Project](/docs/userguide/samples/projects/samples.md), open the level `Samples\ScriptedEntityTweenerSample\SampleFullscreenAnimation`\.
![\[Example level that uses Scripted Entity Tweener.\]](/images/user-guide/ui-animating-tweener-level.png)

1. To switch to gameplay mode, press **Ctrl\+G**\. The header, list items, earth, stars, and spaceship use tweeners for their animation\.
![\[Animated demonstration of the Sample Level for Tweeners\]](/images/user-guide/ui-animating-tweener-uisample.gif)

1. Press **Esc** to exit\.

**To view the sample UI canvas**

1. Open the [**UI Editor**](/docs/user-guide/interactivity/user-interface/editor/using.md)\.

1. Choose **File**, **Open Canvas**\.

1. Navigate to the `lumberyard_version\dev\SamplesProject\UI\Canvases\ScriptedEntityTweenerSample` directory and then select `MenuAnimateInSample.uicanvas`\.

**To view the sample Lua Script with the tweener code**

1. In the **Hierarchy** pane, select **MenuSample**\.

1. In the **Properties** pane, under the **Lua Script** component, for **Script**, click the **\{ \}** icon\. This opens the `samplescreenanimation` script in the **Lua Editor**\.
![\[Select the Lua script for the sample tweener code.\]](/images/user-guide/ui-animating-tweener-sample-1.png)

1. See how the script uses the listed parameters\. For a complete list of parameters, see [Tweener Parameters](/docs/user-guide/user-interface/animating/tweener-parameters.md)\.

   Try the following:
   + Modify the value of the [`easeMethod`](/docs/userguide/ui/animating/tweener-parameters#ui-animating-tweener-easemethod) parameter, especially on the blocks that include `id = self.Properties.CharacterImage`, which represent the spaceship\.
   + Increase or decrease the `duration` parameter\.
   + Change the starting position **\["x"\]** and **\["y"\]**\.

1. Save your changes and close the **Lua Editor**\.

1. In the **UI Editor**, click **Preview** in the upper right to see how your changes affect the game UI\.