# building

workflows etc.

call for a matrix using this template to build java/spark using maven with both profile and scala being profiles (you can swap them for any meaning):

```yaml
  testBuilds_2_12_15_and_13_5:
    uses: sparkutils/building/.github/workflows/run_build.yml@main
    with:
      testString: test
      theMatrix: |
        {
          "profile": [ "Spark32", "Spark321", "Spark332", "Spark341", "Spark350" ],
          "scala": [ "scala_2.12.15", "scala_2.13.5" ]
        }
    secrets: inherit
```

or to publish (need is optional for publish only):

```yaml
  publish_2_12_15_and_13_5:
    needs: [testBuilds_2_12_15_and_13_5, noTestBuilds_2_12_15_and_13_5]
    uses: sparkutils/building/.github/workflows/publish_build.yml@main
    with:
      theMatrix: |
        {
          "profile": [ "Spark32", "Spark321", "Spark332", "Spark341", "Spark350", "10.4.dbr", "11.3.dbr", "12.2.dbr", "13.1.dbr", "13.3.dbr", "14.0.dbr", "14.3.dbr" ],
          "scala": [ "scala_2.12.15", "scala_2.13.5" ]
        }
    secrets: inherit
```

secrets: inherit is needed with the following secrets:

* MAVEN_GPG_PASSPHRASE
* MAVEN_GPG_PRIVATE_KEY
* OSSRH_TOKEN
* OSSRH_USERNAME

which are used by the java action.

run_build takes a main matrix.profile and matrix.scala, run_build_3 adds a matrix.third option, as does publish_build_3 (third for testless is the frameless version).
