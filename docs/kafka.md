# Session 5: Kafka on GKE

## Topic description while broker 1 was down

Topic: ocean-events  PartitionCount: 3  ReplicationFactor: 3  Configs: min.insync.replicas=2,retention.ms=2592000000
  Partition: 0  Leader: 2  Replicas: 2,0,1  Isr: 2,0
  Partition: 1  Leader: 0  Replicas: 0,1,2  Isr: 0,2
  Partition: 2  Leader: 2  Replicas: 1,2,0  Isr: 2,0

## Consumer group ops-dashboard before the reset

GROUP          TOPIC         PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG
ops-dashboard  ocean-events  0          0               296             296
ops-dashboard  ocean-events  1          0               404             404
ops-dashboard  ocean-events  2          50              308             258

## Consumer group ops-dashboard after the reset (--to-earliest)

GROUP          TOPIC         PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG
ops-dashboard  ocean-events  0          0               456             456
ops-dashboard  ocean-events  1          0               565             565
ops-dashboard  ocean-events  2          0               513             513

## Which partition did my shipment land on?

My shipment LIVE-SHP01-000011 landed on partition 2, and all six of its events (BOOKED, DELAYED, GATE_IN, LOADED, DEPARTED, ARRIVED) were there. The producer uses the shipment ID as the message key, and Kafka hashes the key and takes it modulo the number of partitions, so the same key always maps to the same partition. That keeps one shipment's events in order, even though there is no ordering across partitions.

## What happened when the broker died?

When I deleted pod freight-dual-1, broker 1 dropped out of the Isr of every partition, and partition 2, which it led, got a new leader (broker 2). The two remaining replicas were still in sync, which satisfies min.insync.replicas=2, so writes were still accepted and the producer never failed. Its log kept showing events sent (1220 right after the kill). After roughly a minute broker 1 came back, caught up and rejoined the Isr (0,1,2) without anyone intervening.
