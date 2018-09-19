# Understanding actions and the feedback cycle to feed React components using Redux

The developer Tal [1](https://hackernoon.com/redux-step-by-step-a-simple-and-robust-workflow-for-real-life-apps-1fdf7df46092) makes a distinction between (dumb) Components and (smart) Containers, the first having no knowledge of Redux (relationship with the store update and state propagation) but the second does have. Therefore, smart components are the solution for developers when it is time to make user events to kick logic. However, in the flux architecture, these smart components code should not really incorporate any logic in it, according to Tal:

    Rule: Smart components are not allowed to have any logic except dispatching actions.

## Within actions: what kinds of actions are possible?

In the example by Tal, a component does call an action. Let's look at his example:

```
import _ from 'lodash';
import * as types from './actionTypes';
import redditService from '../../services/reddit';

export function fetchTopics() {
  return async(dispatch, getState) => {
    try {
      const subredditArray = await redditService.getDefaultSubreddits();
      const topicsByUrl = _.keyBy(subredditArray, (subreddit) => subreddit.url);
      dispatch({ type: types.TOPICS_FETCHED, topicsByUrl });
    } catch (error) {
      console.error(error);
    }
  };
}
```

You may notice that the above expression "except dispatching actions" may actually refer to multiple things happening related to the action. If you look at the above code, you will notice that the code calls a side function in order to perform an asynchronous operation. If the result is success, then an actual dispatch function is called.   

In the Redux architecture the dispatch is a method for actually dispatching actions. So here we need to make a distinction and understand that the above example refers to an action in the general sense which embodies first the command to start certain logic operations that will depend on external factors and may generate (dispatch) actions as response; thus enabling the store system to feedback the interface eventually when actions are processed by reducers as we will see.
