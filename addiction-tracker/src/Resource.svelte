<script>
    export let title;
    export let url;
    export let blurb;
    export let imageUrl = ''; // Placeholder for the image URL
  </script>
  
  <style>
    .resource {
      margin-bottom: 20px; /* Increase margin between resources */
      padding: 20px;
      background-color: #fff; /* Solid background */
      border: 1px solid #ddd;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Add shadow */
      display: flex;
      align-items: flex-start;
      transition: transform 0.2s; /* Add transition for hover effect */
    }
    .resource:hover {
      transform: translateY(-5px); /* Lift on hover */
    }
    .resource a {
      text-decoration: none;
      color: #007BFF;
    }
    .resource a:hover {
      text-decoration: underline;
    }
    .resource-image {
      width: 120px; /* Increase image size */
      height: 120px;
      object-fit: cover;
      margin-right: 20px; /* Increase margin */
      border-radius: 10px;
    }
    .resource-content {
      flex: 1;
    }
    .resource-content h3 {
      margin: 0 0 10px 0; /* Add margin to the title */
      font-size: 1.5em; /* Increase font size */
    }
    .resource-content p {
      margin: 0 0 10px 0; /* Add margin to the blurb */
      color: #555; /* Change text color */
    }
  </style>
  
  <div class="resource">
    {#if imageUrl}
      <img src={imageUrl} alt={title} class="resource-image" />
    {/if}
    <div class="resource-content">
      <h3>{title}</h3>
      <p>{blurb}</p>
      <a href={url} target="_blank">{url}</a>
    </div>
  </div>