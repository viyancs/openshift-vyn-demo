# openshift-vyn-demo
====================
1. create statefulset mongodb
<pre>
kind: StatefulSet
apiVersion: apps/v1
metadata:
  name: mongo
  namespace: viyancs-test
  selfLink: /apis/apps/v1/namespaces/viyancs-test/statefulsets/mongo
  uid: 762b8800-0b22-4111-9319-ebb212278509
  resourceVersion: '77312609'
  generation: 1
  creationTimestamp: '2021-05-21T13:00:55Z'
  managedFields:
    - manager: Mozilla
      operation: Update
      apiVersion: apps/v1
      time: '2021-05-21T13:00:55Z'
      fieldsType: FieldsV1
      fieldsV1:
        'f:spec':
          'f:podManagementPolicy': {}
          'f:replicas': {}
          'f:revisionHistoryLimit': {}
          'f:selector':
            'f:matchLabels':
              .: {}
              'f:app': {}
          'f:serviceName': {}
          'f:template':
            'f:metadata':
              'f:labels':
                .: {}
                'f:app': {}
                'f:env': {}
                'f:replicaset': {}
                'f:role': {}
            'f:spec':
              'f:affinity':
                .: {}
                'f:podAntiAffinity':
                  .: {}
                  'f:preferredDuringSchedulingIgnoredDuringExecution': {}
              'f:containers':
                'k:{"name":"mongo"}':
                  'f:image': {}
                  'f:volumeMounts':
                    .: {}
                    'k:{"mountPath":"/data/db"}':
                      .: {}
                      'f:mountPath': {}
                      'f:name': {}
                  'f:terminationMessagePolicy': {}
                  .: {}
                  'f:resources': {}
                  'f:command': {}
                  'f:terminationMessagePath': {}
                  'f:imagePullPolicy': {}
                  'f:ports':
                    .: {}
                    'k:{"containerPort":27017,"protocol":"TCP"}':
                      .: {}
                      'f:containerPort': {}
                      'f:protocol': {}
                  'f:name': {}
              'f:dnsPolicy': {}
              'f:restartPolicy': {}
              'f:schedulerName': {}
              'f:securityContext': {}
              'f:terminationGracePeriodSeconds': {}
          'f:updateStrategy':
            'f:rollingUpdate':
              .: {}
              'f:partition': {}
            'f:type': {}
          'f:volumeClaimTemplates': {}
    - manager: kube-controller-manager
      operation: Update
      apiVersion: apps/v1
      time: '2021-05-21T13:00:55Z'
      fieldsType: FieldsV1
      fieldsV1:
        'f:status':
          'f:collisionCount': {}
          'f:currentReplicas': {}
          'f:currentRevision': {}
          'f:observedGeneration': {}
          'f:replicas': {}
          'f:updateRevision': {}
          'f:updatedReplicas': {}
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: mongo
        env: demo
        replicaset: rs0.main
        role: db
    spec:
      containers:
        - resources: {}
          terminationMessagePath: /dev/termination-log
          name: mongo
          command:
            - numactl
            - '--interleave=all'
            - mongod
            - '--wiredTigerCacheSizeGB'
            - '0.1'
            - '--bind_ip'
            - 0.0.0.0
            - '--replSet'
            - rs0
          ports:
            - containerPort: 27017
              protocol: TCP
          imagePullPolicy: Always
          volumeMounts:
            - name: mongodb-persistent-storage-claim
              mountPath: /data/db
          terminationMessagePolicy: File
          image: mongo
      restartPolicy: Always
      terminationGracePeriodSeconds: 10
      dnsPolicy: ClusterFirst
      securityContext: {}
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                labelSelector:
                  matchExpressions:
                    - key: replicaset
                      operator: In
                      values:
                        - rs0.main
                topologyKey: kubernetes.io/hostname
      schedulerName: default-scheduler
  volumeClaimTemplates:
    - kind: PersistentVolumeClaim
      apiVersion: v1
      metadata:
        name: mongodb-persistent-storage-claim
        creationTimestamp: null
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 512Mi
        storageClassName: ebs
        volumeMode: Filesystem
      status:
        phase: Pending
  serviceName: mongo
  podManagementPolicy: OrderedReady
  updateStrategy:
    type: RollingUpdate
    rollingUpdate:
      partition: 0
  revisionHistoryLimit: 10
status:
  observedGeneration: 1
  replicas: 1
  currentReplicas: 1
  updatedReplicas: 1
  currentRevision: mongo-9bf5cf86
  updateRevision: mongo-9bf5cf86
  collisionCount: 0

</pre>
