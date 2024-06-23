### Pagination in Python

Pagination is essential when working with large datasets to divide the data into manageable chunks or pages. Let's explore how to implement pagination in Python, covering simple pagination, pagination with hypermedia metadata, and pagination that is resilient to deletions.

#### 1. Simple Pagination

Simple pagination involves dividing a dataset into pages using `page` and `page_size` parameters.

```python
#!/usr/bin/env python3
"""
Simple Pagination Example
"""

def paginate(dataset, page, page_size):
    """
    Paginate a dataset with given page number and page size.

    Args:
        dataset (list): The list of items to paginate.
        page (int): The page number (1-indexed).
        page_size (int): The number of items per page.

    Returns:
        list: The paginated dataset for the given page.
    """
    start_index = (page - 1) * page_size
    end_index = start_index + page_size
    return dataset[start_index:end_index]

# Example usage
dataset = [i for i in range(1, 101)]  # Dataset with 100 items
page = 2
page_size = 10
print(paginate(dataset, page, page_size))
```

#### 2. Pagination with Hypermedia Metadata

Hypermedia metadata includes additional information such as total items, total pages, current page, and next/previous page links.

```python
#!/usr/bin/env python3
"""
Pagination with Hypermedia Metadata Example
"""

def paginate_with_metadata(dataset, page, page_size):
    """
    Paginate a dataset with hypermedia metadata.

    Args:
        dataset (list): The list of items to paginate.
        page (int): The page number (1-indexed).
        page_size (int): The number of items per page.

    Returns:
        dict: A dictionary containing the paginated dataset and metadata.
    """
    total_items = len(dataset)
    total_pages = (total_items + page_size - 1) // page_size  # Ceiling division
    start_index = (page - 1) * page_size
    end_index = start_index + page_size
    paginated_data = dataset[start_index:end_index]

    metadata = {
        'page': page,
        'page_size': page_size,
        'total_items': total_items,
        'total_pages': total_pages,
        'data': paginated_data,
        'next_page': page + 1 if page < total_pages else None,
        'previous_page': page - 1 if page > 1 else None
    }
    return metadata

# Example usage
dataset = [i for i in range(1, 101)]  # Dataset with 100 items
page = 2
page_size = 10
print(paginate_with_metadata(dataset, page, page_size))
```

#### 3. Deletion-Resilient Pagination

Deletion-resilient pagination ensures the pagination mechanism is robust even when items are deleted from the dataset.

```python
#!/usr/bin/env python3
"""
Deletion-Resilient Pagination Example
"""

def deletion_resilient_paginate(dataset, page, page_size):
    """
    Paginate a dataset in a deletion-resilient manner.

    Args:
        dataset (list): The list of items to paginate.
        page (int): The page number (1-indexed).
        page_size (int): The number of items per page.

    Returns:
        dict: A dictionary containing the paginated dataset and metadata.
    """
    # Filter out None values or any deleted markers
    filtered_dataset = [item for item in dataset if item is not None]
    total_items = len(filtered_dataset)
    total_pages = (total_items + page_size - 1) // page_size  # Ceiling division
    start_index = (page - 1) * page_size
    end_index = start_index + page_size
    paginated_data = filtered_dataset[start_index:end_index]

    metadata = {
        'page': page,
        'page_size': page_size,
        'total_items': total_items,
        'total_pages': total_pages,
        'data': paginated_data,
        'next_page': page + 1 if page < total_pages else None,
        'previous_page': page - 1 if page > 1 else None
    }
    return metadata

# Example usage
dataset = [i for i in range(1, 101)]  # Dataset with 100 items
# Simulate deletions
dataset[5] = None  # Deleting the 6th item
dataset[20] = None  # Deleting the 21st item
page = 2
page_size = 10
print(deletion_resilient_paginate(dataset, page, page_size))
```

### Summary

- **Simple Pagination**: Divides a dataset into pages using `page` and `page_size`.
- **Hypermedia Metadata Pagination**: Provides additional information such as total items, total pages, current page, and navigation links.
- **Deletion-Resilient Pagination**: Handles pagination even when items are deleted, ensuring the mechanism remains robust. 

These examples provide a foundation for implementing pagination in various scenarios. Adjustments can be made based on specific requirements and dataset structures.
