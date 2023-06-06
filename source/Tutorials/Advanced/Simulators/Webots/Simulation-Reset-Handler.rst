Setting up a Reset Handler
==========================

**Goal:** Extend a robot simulation with a reset handler to restart nodes when the reset button of Webots is pressed.

**Tutorial level:** Advanced

**Time:** 20 minutes

.. contents:: Contents
   :depth: 2
   :local:

Background
----------

In this tutorial, you will learn how to implement a reset handler in a robot simulation using Webots. 
The Webots reset button reverts the world to the initial state and restarts controllers. 
It is convenient as it quickly resets the simulation, but in the context of ROS 2, robot controllers are not started again making the simulation stop. 
The reset handler allows you to restart specific nodes or perform additional actions when the reset button in Webots is pressed. 
This can be useful for scenarios where you need to reset the state of your simulation or restart specific components without completely restarting the complete ROS system.

Prerequisites
-------------

Before proceeding with this tutorial, make sure you have completed the following:

- Understanding of ROS 2 nodes and topics covered in the beginner :doc:`../../../../Tutorials`. 
- Knowledge of Webots and ROS 2 and its interface package.
- Familiarity with :doc:`./Setting-Up-Simulation-Webots-Basic`.


Reset Handler for Simple Cases (Controllers Only)
-------------------------------------------------

In the launch file of your package, add the ``respawn`` parameter.

.. code-block:: python

  def generate_launch_description():
      robot_driver = WebotsController(
          robot_name='my_robot',
          parameters=[
              {'robot_description': robot_description_path}
          ],

          # Every time one resets the simulation the controller is automatically respawned
          respawn=True
      )

      # Starts Webots
      webots = WebotsLauncher(world=PathJoinSubstitution([package_dir, 'worlds', world]))

      return LaunchDescription([
          webots,
          robot_driver,
          launch.actions.RegisterEventHandler(
              event_handler=launch.event_handlers.OnProcessExit(
                  target_action=webots,
                  on_exit=[launch.actions.EmitEvent(event=launch.events.Shutdown())],
              )
          )
      ])

On the reset, Webots kills all driver nodes. 
Therefore, to start them again after the reset is done, you can set the ``respawn`` property of the driver node to ``True``. 
It will ensure driver nodes are up and running after the reset.

Reset Handler for Multiple Nodes (No Shutdown Required)
-------------------------------------------------------


Reset Handler Requiring Node Shutdown
-------------------------------------



Summary
-------

In this tutorial, you learned how to implement a reset handler in a robot simulation using Webots. 
The reset handler allows you to restart specific nodes or perform additional actions when the reset button in Webots is pressed. 
You explored different approaches based on the complexity of your simulation and the requirements of your nodes.

Next steps
----------

You might want to improve the plugin or create new nodes to change the behavior of the robot.
Taking inspiration from these previous tutorials could be a starting point:

* :doc:`../../Recording-A-Bag-From-Your-Own-Node-Py`.

* :doc:`../../../Intermediate/Tf2/Tf2-Main`.
