pipeline {
    environment {
        registry = "continuouslee/real-world-backend"
        registryCredential = "dockerhub"
        dockerImage = ""
    }
    agent any
    tools {
        gradle 'gradle621'
        jdk 'jdk8'
    }

    // This shows a simple build wrapper example, using the AnsiColor plugin.
    stage {
        // This displays colors using the 'xterm' ansi color map.
        ansiColor('xterm') {
            // Just some echoes to show the ANSI color.
            stage "\u001B[31mI'm Red\u001B[0m Now not"
        }
    }
}