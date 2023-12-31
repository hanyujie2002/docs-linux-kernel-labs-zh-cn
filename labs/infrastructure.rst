基础设施
=======

为了帮助学习，我们每个主题都有一个实践练习部分，其中包含详细的、逐步的提示，以解决一个或多个任务。为了便于专注于特定问题，大多数任务将在现有的骨架（skeleton）驱动程序上执行。每个骨架驱动程序都有清晰标记的部分，需要填写之以完成任务。

骨架驱动程序是从位于 tools/labs/templates 目录中的完整源代码示例生成的。要解决任务，你可以通过在 *tools/labs* 目录下运行**skels**目标来生成骨架驱动程序。为了保持工作空间的干净，建议一次只为一个实验生成骨架，然后在开始新的实验之前清理工作空间。你可以使用**LABS**变量来选择实验：

.. code-block:: shell

   tools/labs $ make clean
   tools/labs $ LABS=kernel_modules make skels
		
   tools/labs $ ls skels/kernel_modules/
   1-2-test-mod  3-error-mod  4-multi-mod  5-oops-mod  6-cmd-mod  \
   7-list-proc  8-kprobes  9-kdb

你还可以使用相同的变量来生成特定任务的骨架：

.. code-block:: shell

   tools/labs $ LABS="kernel_modules/6-cmd-mod kernel_modules/8-kprobes" make skels
		
   tools/labs$ ls skels/kernel_modules
   6-cmd-mod  8-kprobes


对于每个任务，你可能需要执行多个步骤，这通常是逐步进行的。这些步骤在源代码中以及实验练习中都用关键字*TODO*标记。如果我们有多个步骤要执行，它们将以数字作为前缀，例如 *TODO1*、 *TODO2* 等。如果没有使用数字，则是只有一个步骤。如果你想从某个特定步骤开始执行任务，可以使用 **TODO** 变量。以下示例将生成解决了第一个 *TODO* 步骤的骨架：

.. code-block:: shell

   tools/labs $ TODO=2 LABS="kernel_modules/8-kprobes" skels

生成骨架驱动程序后，可以使用 **build** make 目标来构建它们：

.. code-block:: shell

   tools/labs $ make build
   echo "# 自动生成的，不要编辑 " > skels/Kbuild
   for i in ./kernel_modules/8-kprobes; do echo "obj-m += $i/" >> skels/Kbuild; done
   make -C /home/tavi/src/linux M=/home/tavi/src/linux/tools/labs/skels ARCH=x86 modules
   make[1]: Entering directory '/home/tavi/src/linux'
   CC [M]  /home/tavi/src/linux/tools/labs/skels/./kernel_modules/8-kprobes/kprobes.o
   Building modules, stage 2.
   MODPOST 1 modules
   CC      /home/tavi/src/linux/tools/labs/skels/./kernel_modules/8-kprobes/kprobes.mod.o
   LD [M]  /home/tavi/src/linux/tools/labs/skels/./kernel_modules/8-kprobes/kprobes.ko
   make[1]: Leaving directory '/home/tavi/src/linux'


要将驱动程序复制到虚拟机中，你可以使用 ssh 或直接使用 **copy** 目标来更新虚拟机映像：

.. code-block:: shell

   tools/labs $ make copy
   ...
   'skels/kernel_modules/8-kprobes/kprobes.ko' -> '/tmp/tmp.4UMKcISmQM/home/root/skels/kernel_modules/8-kprobes/kprobes.ko'

.. attention:: 如果虚拟机正在运行，**copy** 目标将失败。这是有意为之，以避免破坏文件系统。
