# File Storage System

traditional method to store data
like a linux file system

data is stored on block sizes
these blocks are kept on hard disk
efficient to read and write

ex: NTFS, FAT32, ext4
NFS
SMB

## Advantages

- Familiar and Intuitive
- Granular Permissions
- Efficient for Small Files

## Disadvantages

- Scalability Limitations
- Limited Metadata
- Complexity with Big Data

# Object Storage

object consist of data, unique identifier, extensive meta-data
retrive via id

```
Data: [Binary Data]
[Object ID: 123abc]
Metadata: {Key-Value Pairs}
```

stored in flat address space{storage pool}

```
[Object ID: 01]
[Object ID: 02]
[Object ID: 031]
    ...
[Object ID: NI
```

ex: AWS S3,

## Pros

- Highly Scalable
- Rich Metadata
- Cost Effective for Big Data

## Cons

- Less Suitable for Small Files
- No File Hierarchy
- Higher Latency for Some Ops

## When to use What

| File Storage               | Object Storage             |
| -------------------------- | -------------------------- |
| Need Hierarchial Structure | Handling large DataSet     |
| Working with small files   | Need rich Metadata         |
| Rich Granular permission   | Optimising for Scalability |

# Real World

Company Shared Drive

Cloud Storage for Media Content
[Object IDs]: - vid123abc - image456def - audio_123

Metadata: - Title, Format, Resolution - Duration, Tags, Creator

![sample code](https://github.com/NalinDalal/file-storage-rust)
