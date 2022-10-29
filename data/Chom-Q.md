## You should take adjusted_balance_of into account on calculation votesDifference on createPledge

Although you don't take it into account, the system still has some problems with votes decay / expire during the Pledge duration as stated in my Medium findings. So, you should take adjusted_balance_of into account to get the actual voting difference.