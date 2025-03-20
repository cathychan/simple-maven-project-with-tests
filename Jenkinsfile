podTemplate(containers: [containerTemplate(name: 'maven', image: 'maven', command: 'sleep', args: 'infinity')]) {
  node(POD_LABEL) {
    checkout scm
    container('maven') {
      sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
    }
    junit(testResults: '**/target/surefire-reports/TEST-*.xml',
          testDataPublishers: [
            jiraTestResultReporter(
              configs: [
                jiraStringField(fieldKey: 'summary', value: 'TEST ONLY - IGNORE'),
                jiraStringField(fieldKey: 'description', value: 'Test ticket for JiraTestResultReporter. Ignore')
              ],
              projectKey: 'BEE',
              issueType: '11185', // automated test failure
              autoRaiseIssue: true,
              autoResolveIssue: true,
              autoUnlinkIssue: false,
              overrideResolvedIssues: true
            )
          ])
    archiveArtifacts artifacts: '**/target/surefire-reports/TEST-*.xml', allowEmptyArchive: true
  }
}
