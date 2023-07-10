# Running Kotlin on our Robot

Java has been a popular language in the First Robotics Competition and has 
large support with wpilib. However, Java has been criticized for verbosity
and complexity. However, our team considers the wpilib version for Java to
have the best support and is likely the easiest one to use for the Robot.

However, considering Java's shortcomings, it would be nice to use a more
modern language with more concise syntax that has less boilerplate. In fact,
such a language does exist: Kotlin!

Created by JetBrains (the same people responsible for the PyCharm and IntelliJ
IDES), it aims to be a more modern version of Java with null safety and cleaner
syntax while still being able to take advantage of Java libraries.

While Kotlin isn't a popular language for FRC so getting help about Kotlin
specific topics at competitions will be difficult, some have already made
the attempt to program their robot in Kotlin such as team 4069 and team 2898
([Their Chief Delpi Post](https://www.chiefdelphi.com/t/paper-kotlin-for-frc-robot-programming/166496)).

However, I was moderately interested in seeing how difficult it would be
to program the robot that we built in Kotlin so I did a little research at some
meetings and it actually turned out to be very painless!

Firstly, I downloaded the [Kotlin for FRC](https://marketplace.visualstudio.com/items?itemName=Brenek.kotlin-for-frc)
VSCode extension for the wpilib version of VSCode and then created a Java project
for the robot. I then used the extension to convert the project to Kotlin where
it provided a basic template for me to use.

As Kotlin is able to work almost perfectly with Java code, all function
and class names were the same as the Java and I was able to just download 
the wpilib Java library and reference the wpilib documentation and while I had
to make some syntax changes for Kotlin, it still had the same ideas. In fact,
I was even able to download the Java version REVLib and use it easily with
Kotlin!

I started by first creating a simple project by following the REVLib and
wpilib documentation to make the robot rotate and move at the same time and was
able to write the following program for the robot:

{% highlight kotlin %}
package frc.robot

import edu.wpi.first.wpilibj.TimedRobot
import edu.wpi.first.wpilibj.smartdashboard.SendableChooser
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.drive.MecanumDrive;
import com.revrobotics.CANSparkMax;
import com.revrobotics.CANSparkMaxLowLevel.MotorType;

/**
 * The VM is configured to automatically run this class, and to call the
 * functions corresponding to each mode, as described in the TimedRobot
 * documentation. If you change the name of this class or the package after
 * creating this project, you must also update the build.gradle file in the
 * project.
 */

class Robot : TimedRobot() {
    private var autonomousChooser = SendableChooser<String>()
    private val m_timer = Timer()

    private val frontLeftChannel: Int = 1
    private val rearLeftChannel: Int = 3
    private val frontRightChannel: Int = 2
    private val rearRightChannel: Int = 4
    private val frontLeft = CANSparkMax(frontLeftChannel, MotorType.kBrushless)
    private val rearLeft = CANSparkMax(rearLeftChannel, MotorType.kBrushless)
    private val frontRight = CANSparkMax(frontRightChannel, MotorType.kBrushless)
    private val rearRight = CANSparkMax(rearRightChannel, MotorType.kBrushless)

    private val m_robotDrive = MecanumDrive(frontLeft, rearLeft, frontRight, rearRight) 

    companion object {
        private const val DEFAULT_AUTO = "Default"
        private const val CUSTOM_AUTO = "My Auto"
    }

    /**
     * This function is run when the robot is first started up and should be
     * used for any initialization code.
     */
    override fun robotInit() {
        autonomousChooser.setDefaultOption("Default Auto", DEFAULT_AUTO)
        autonomousChooser.addOption("My Auto", CUSTOM_AUTO)
        SmartDashboard.putData("Auto choices", autonomousChooser)
    }

    /**
     * This autonomous (along with the chooser code above) shows how to select
     * between different autonomous modes using the dashboard. The sendable
     * chooser code works with the Java SmartDashboard. If you prefer the
     * LabVIEW Dashboard, remove all of the chooser code and uncomment the
     * getString line to get the auto name from the text box below the Gyro
     *
     *
     * You can add additional auto modes by adding additional comparisons to
     * the switch structure below with additional strings. If using the
     * SendableChooser make sure to add them to the chooser code above as well.
     */
    override fun autonomousInit() {
        m_timer.start()
        m_robotDrive.setSafetyEnabled(false)
    }

    /**
     * This function is called periodically during autonomous.
     */
    override fun autonomousPeriodic() {
        println("Hello - this is autonomus")
        //Drive forward for two seconds
        if (m_timer.get() < 2.0) {
            println("Should be driving")
            m_robotDrive.driveCartesian(0.2, 0.2, 0.2)
        } 
    }
}

{% endhighlight %}

I was also able to write some code to move the robot with an XBox controller
in Kotlin but I currently don't have a copy of the code at the moment
so I will update this post with the code example when I get the chance.

Overall, I had a very good experience with Kotlin, I like the syntax, it works
seamlessly with Java libraries and fixes many of Java's shortcomings. While our
team probably is not using Kotlin for this year, I do hope to see Kotlin's future
success in the FIRST Robotics world. In my opinion, this was a worthwhile
experiment.
