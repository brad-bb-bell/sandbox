<template>
  <main class="px-8">
    <!-- Display categorized activities -->
    <div v-for="category in categories" :key="category.id">
      <h2 class="py-4">{{ category.name }}</h2>

      <draggable
        v-model="category.activities"
        itemKey="id"
        tag="ol"
        group="activities"
        @start="dragStart"
        @end="dropItem"
        :data-category-id="category.id"
      >
        <template #item="{ element }">
          <li class="cursor-pointer">{{ element.name }}</li>
        </template>
      </draggable>
    </div>

    <!-- Display uncategorized activities -->
    <div v-if="uncategorizedActivities.length">
      <h2 class="py-4">Uncategorized</h2>
      <draggable
        v-model="uncategorizedActivities"
        itemKey="id"
        tag="ol"
        group="activities"
        @start="dragStart"
        @end="dropItem"
      >
        <template #item="{ element }">
          <li class="cursor-pointer" :data-id="element.id">{{ element.name }}</li>
        </template>
      </draggable>
    </div>
  </main>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  data() {
    return {
      activities: [
        {
          id: 1,
          name: "yoga",
          categories: [],
        },
        {
          id: 3,
          name: "skateboard",
          categories: [],
        },
        {
          id: 4,
          name: "bicycle",
          categories: [],
        },
        {
          id: 8,
          name: "gym climb",
          categories: [
            {
              id: 3,
              name: "climbing",
            },
          ],
        },
        {
          id: 10,
          name: "rock climb",
          categories: [
            {
              id: 3,
              name: "climbing",
            },
          ],
        },
        {
          id: 11,
          name: "peloton",
          categories: [],
        },
        {
          id: 5,
          name: "backcountry ski",
          categories: [
            {
              id: 1,
              name: "ski",
            },
          ],
        },
        {
          id: 6,
          name: "ski big sky",
          categories: [
            {
              id: 1,
              name: "ski",
            },
          ],
        },
        {
          id: 7,
          name: "ski bridger bowl",
          categories: [
            {
              id: 1,
              name: "ski",
            },
          ],
        },
        {
          id: 12,
          name: "peak bagging",
          categories: [
            {
              id: 2,
              name: "hiking",
            },
          ],
        },
        {
          id: 2,
          name: "hike the M",
          categories: [
            {
              id: 2,
              name: "hiking",
            },
          ],
        },
        {
          id: 9,
          name: "regular ol hike",
          categories: [
            {
              id: 2,
              name: "hiking",
            },
          ],
        },
        {
          id: 13,
          name: "backpacking",
          categories: [
            {
              id: 2,
              name: "hiking",
            },
          ],
        },
      ],
      categories: [],
      uncategorizedActivities: [],
      draggedItem: null,
    };
  },
  methods: {
    dragStart(e) {
      this.draggedItem = e.item;
      console.log("activityId of dragged item", e.item.dataset.id);
    },
    dropItem(e) {
      console.log("categoryId of dropped item", e.to.dataset.categoryId);
      // console.log("activityID", e.to.dataset.activityId);
      // // const toCategoryID = e.to.dataset.categoryId;
      // console.log("dropItem", e, toCategoryID);
      // this.addActivityToCategory(this.draggedItem.id, toCategoryID);
    },
    addActivityToCategory(activityID, categoryID) {
      // Find the activity and category objects
      const activity = this.activities.find((a) => a.id === activityID);
      const category = this.categories.find((c) => c.id === parseInt(categoryID));

      if (activity && category) {
        // Remove from previous category if it exists
        activity.categories.forEach((cat, index) => {
          if (cat.id === category.id) {
            activity.categories.splice(index, 1);
          }
        });

        // Add to the new category
        activity.categories.push({ id: category.id, name: category.name });
        // You may want to do a Vue.set if you need reactivity
      }
    },
    detectMove(e) {
      console.log("detectMove", e);
    },
    saveOrder() {
      console.log("saveOrder");
    },
    getCategories() {
      let categoriesMap = {};
      this.uncategorizedActivities = []; // New array for activities without categories

      // Loop through all activities
      this.activities.forEach((activity) => {
        // Check if the activity has categories
        if (activity.categories && activity.categories.length > 0) {
          // Loop through all categories of the activity
          activity.categories.forEach((category) => {
            // If the category is already in the categoriesMap, push the activity to its list
            if (categoriesMap[category.id]) {
              categoriesMap[category.id].activities.push(activity);
            } else {
              // Otherwise, create a new category in the categoriesMap with the activity
              categoriesMap[category.id] = {
                ...category,
                activities: [activity],
              };
            }
          });
        } else {
          // If the activity has no categories, add it to the uncategorizedActivities array
          this.uncategorizedActivities.push(activity);
        }
      });

      // Convert the categoriesMap object to an array of values (which are the categories)
      this.categories = Object.values(categoriesMap);
      console.log("categories", this.categories);
      console.log("uncategorizedActivities", this.uncategorizedActivities);
    },
  },
  mounted() {
    this.getCategories();
  },
};
</script>
