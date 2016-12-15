---
layout: post
title: "Data Binding (ListView)"
date: 2015-10-03 20:09:00 
comments: true
categories: []
tags: []
description: "Data Binding (ListView)"
keywords: 
---


 
  public static class SimpleAdapter extends BaseAdapter {

        @Override
        public int getCount() {
            return 0;
        }

        @Override
        public Object getItem(int position) {
            return null;
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            ListItemBinding binding = null;
            if (convertView == null) {
                binding = DataBindingUtil.inflate(LayoutInflater.from(mContext), R.layout.list_item, parent, false);
                convertView = binding.getRoot();
                convertView.setTag(binding);
            } else {
                binding = (ListItemBinding) convertView.getTag();
            }

            binding.setAdapter(this);
            return convertView;
        }
    }
 
 
  $(function () {
                $('pre.prettyprint code').each(function () {
                    var lines = $(this).text().split('\n').length;
                    var $numbering = $('').addClass('pre-numbering').hide();
                    $(this).addClass('has-numbering').parent().append($numbering);
                    for (i = 1; i ').text(i));
                    };
                    $numbering.fadeIn(1700);
                });
            });
 


