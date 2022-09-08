## Policy related CRDs

For simple policies like add header, remove header, add query param use K8s gateway filters or `RequestHeaderModifier` defined in envoy gateway. Refer `/examples/RequestHeaderModifier`.

For more advanced use cases, follow one of the following custom policy formats defined in `wso2.com_policies.yaml`.

### 1. Policy defined using parameters.

Can use any arbitrary number of parameters.

```yaml
apiVersion: wso2.com/v1beta1
kind: Policy
metadata:
  name: custom-policy-1
  namespace: wso2
spec:
  type: CUSTOM_POLICY_1
  parameters:
    - parameterName: x-wso2-custom-header
      parameterValue: foo
```

### 2. Policy defined using a json scheme.

Can use any arbitrary json.

```yaml
apiVersion: wso2.com/v1beta1
kind: Policy
metadata:
  name: custom-policy-2
  namespace: wso2
spec:
  type: CUSTOM_POLICY_2
  policySpec:
    json:
      foo: fooVal
      bar: barVal
      baz:
        baz1: baz1
        baz2:
          foobaz: foobaz
          foobar: foobar
```

### 3. Policy defined using a string template.

```yaml
apiVersion: wso2.com/v1beta1
kind: Policy
metadata:
  name: custom-policy-3
  namespace: wso2
spec:
  type: CUSTOM_POLICY_3
  template: |
    custom policy template starts here
      define custom policy here
    custom policy template ends here
```
