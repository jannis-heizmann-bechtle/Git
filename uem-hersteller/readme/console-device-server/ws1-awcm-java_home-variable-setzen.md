# WS1 - AWCM: JAVA\_HOME Variable setzen

Der AWCM Service basiert auf JAVA. Manchmal wird die JAVA\_HOME Variable im Betriebssystem nicht richtig gesetzt und diese muss manuell gesetzt werden. Dies geht wie folgt:&#x20;

After you’ve [installed Java](https://confluence.atlassian.com/doc/installing-java-for-confluence-144212101.html) in Windows, you must set the  `JAVA_HOME`  environment variable to point to the Java installation directory.

This information is only relevant if you’re installing Confluence manually on a Windows server. If you’re using the installer, you don’t need to do this.

If you installed the Java Development Kit (JDK) you’ll be setting the `JAVA_HOME` environment variable. If you installed the Java Runtime Environment (JRE) you will follow the same steps but set the `JRE_HOME` environment variable instead.&#x20;

### Set the JAVA\_HOME Variable <a href="#settingthejava_homevariableinwindows-setthejava_homevariable" id="settingthejava_homevariableinwindows-setthejava_homevariable"></a>

To set the JAVA\_HOME variable:

1. Locate your Java installation directoryIf you didn’t change the path during installation, it’ll be something like `C:\Program Files\Java\jdk1.8.0_65`You can also type `where java` at the command prompt.
2. Do one of the following:\
   **Windows 7** – Right click **My Computer** and select **Properties** > **Advanced** \
   **Windows 8** – Go to **Control Panel** > **System** > **Advanced System Settings**\
   **Windows 10** – Search for **Environment Variables** then select **Edit the system environment variables**
3. Click the **Environment Variables** button.
4. Under **System Variables**, click **New**.
5. In the **Variable Name** field, enter either:
   * `JAVA_HOME` if you installed the JDK (Java Development Kit)\
     or`JRE_HOME` if you installed the JRE (Java Runtime Environment)&#x20;
6. In the **Variable Value** field, enter your JDK or JRE installation path .If the path contains spaces, use the shortened path name. For example, `C:\Progra~1\Java\jdk1.8.0_65`\
   &#x20;Note for Windows users on 64-bit systemsProgra\~1 = ‚Program Files‘\
   Progra\~2 = ‚Program Files(x86)‘
7. Click **OK** and **Apply Changes** as prompted

You’ll need to close and re-open any command windows that were open before you made these changes, as there’s no way to reload environment variables from an active command prompt. If the changes don’t take effect after reopening the command window, restart Windows.

### Set the JAVA\_HOME variable via the command line <a href="#settingthejava_homevariableinwindows-setthejava_homevariableviathecommandline" id="settingthejava_homevariableinwindows-setthejava_homevariableviathecommandline"></a>

If you would prefer to set the JAVA\_HOME (or JRE\_HOME) variable via the command line:

1. Open Command Prompt (make sure you Run as administrator so you’re able to add a system environment variable).
2. Set the value of the environment variable to your JDK (or JRE) installation path as follows:`setx -m JAVA_HOME "C:\Progra~1\Java\jdk1.8.0_XX"`If the path contains spaces, use the shortened path name.
3. Restart Command Prompt to reload the environment variables then use the following command to check the it’s been added correctly. `echo %JAVA_HOME%`You should see the path to your JDK (or JRE) installation.&#x20;
