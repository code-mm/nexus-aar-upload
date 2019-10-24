# 将aar库上传到nexus



## 在gradle.properties 添加配置

    ####################################################################################################
    #服务器地址
    maven_snapshot_url=http://nexus.mhw828.com:8081/repository/maven-snapshots/
    maven_releases_url=http://nexus.mhw828.com:8081/repository/maven-releases/
    ####################################################################################################
    #用户名密码
    maven_local_username=module
    maven_local_password=module
    ####################################################################################################
    #组
    maven_pom_groupid=com.ms
    ####################################################################################################
    #模块名称
    maven_pom_artifactid=module-test
    ####################################################################################################
    #类型
    maven_pom_packaging=aar
    aar=aar
    jar=jar
    ####################################################################################################
    #简介
    maven_pom_description=
    ####################################################################################################
    #  版本管理
    snapshot_versionname=laster-SNAPSHOT
    versionname=laster-release
    ####################################################################################################



## 添加插件

apply plugin: 'maven'



## 配置任务

uploadArchives {
    repositories.mavenDeployer {
        repository(url: maven_releases_url) {
            authentication(userName: maven_local_username, password: maven_local_password)
        }

        snapshotRepository(url: maven_snapshot_url) {
            authentication(userName: maven_local_username, password: maven_local_password)
        }

        pom.project {
            // 注意：【这里通过切换 versionName 的赋值来区分上传快照包还是正式包（snapshot 版本必须以 -SNAPSHOT 结尾）】
            //version snapshotVersionName
            //组 maven_pom_groupid
            groupId maven_pom_groupid
            //项目名称
            artifactId maven_pom_artifactid
            //  类型
            packaging aar
            // 描述
            description maven_pom_description
            // 版本
            version versionname

        }
    }
}

## 运行 uploadArchives

    ./gradlew uploadArchives